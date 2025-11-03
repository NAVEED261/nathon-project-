# NATHON Project - Automated Order Management Workflow

## Project Overview

The "NATHON Project" is an robust automation workflow designed using **n8n**, a powerful open-source workflow automation platform. This project focuses on streamlining order management processes, from fetching real-time order data from an external ERP system to organizing it within Airtable and providing instant notifications on Discord. It's built to enhance efficiency and ensure that critical order information, especially those in "processing" status, are accurately tracked, transformed, and communicated across relevant platforms. This automated solution significantly reduces manual intervention, minimizes errors, and accelerates the informational flow crucial for dynamic business operations. The "level one test has been passed" suggests a foundational stability and readiness for further development or deployment in a production environment, marking a significant milestone in its development.

## Key Features

This n8n workflow encapsulates a suite of functionalities to create a comprehensive order management pipeline:

*   **Scheduled Execution**: The entire workflow is set to run automatically on a predefined schedule, ensuring timely processing of order data without requiring manual initiation. This guarantees that your systems are always updated with the latest information.
*   **ERP Integration**: Seamlessly connects with an external ERP (Enterprise Resource Planning) system via a custom webhook, allowing for the retrieval of new or updated order information. This integration ensures that the workflow acts on the most current operational data.
*   **Conditional Processing**: Incorporates intelligent conditional logic to filter and process orders based on their status. Specifically, it identifies and prioritizes orders that are in a "processing" state, allowing for targeted actions.
*   **Airtable Database Management**: Automatically creates and updates records within a dedicated Airtable base ("NATHON_ORDERS") and table ("ALL_ORDERS"). This provides a flexible and centralized database solution for storing detailed order information, including critical fields like `orderID`, `customerID`, `employeeName`, `orderPrice`, and `orderStatus`.
*   **Data Aggregation and Transformation**: Utilizes a custom JavaScript code node to aggregate and transform order data dynamically. This includes calculating key metrics such as the total number of booked orders (`totalBooked`) and their cumulative value (`bookedSum`), providing valuable business insights.
*   **Discord Notifications**: Delivers real-time, summarized notifications to a specified Discord channel. These alerts include statistics on newly booked orders and their total value, along with unique execution IDs for traceability, keeping relevant teams immediately informed of key operational updates.

## How the Workflow Works (Detailed Breakdown)

The `NATHON WORKFLOW.json` defines a clear, step-by-step automation process:

1.  **Schedule Trigger**:
    *   **Purpose**: Initiates the entire workflow.
    *   **Configuration**: Configured to run every week, specifically on Monday at 9:00 AM. This ensures a consistent, automated check and processing cycle for order data.

2.  **HTTP Request (ERP Data Fetch)**:
    *   **Purpose**: To retrieve order data from your internal ERP system.
    *   **Configuration**: Sends an authenticated HTTP request to `https://internal.users.n8n.cloud/webhook/custom-erp`. Authentication is handled via a generic HTTP header, including a `unique_id` (`27bb5bb4e1d5e2a236b725e6b6a54c05`) for secure and identifiable communication.

3.  **If Node (Order Status Filter)**:
    *   **Purpose**: Implements conditional branching based on the received order data.
    *   **Configuration**: Evaluates the `orderStatus` field from the incoming JSON data. If an order's status exactly matches "processing" (case-sensitive), the workflow proceeds through one path; otherwise, it takes an alternative path (implied by the connections).

4.  **Edit Fields (Data Preparation)**:
    *   **Purpose**: To extract and prepare specific order information before Airtable insertion.
    *   **Configuration**: This node takes the data from the previous steps and specifically extracts `orderID` and `orderPrice` from the JSON payload, preparing them for subsequent actions.

5.  **Create a record (Airtable Integration)**:
    *   **Purpose**: To persistently store and manage order details in Airtable.
    *   **Configuration**: Connects to an Airtable base (`app6dhXur0JfpCXv1`) named "NATHON_ORDERS" and a table (`tblPvTFKgcGL8MPTN`) named "ALL_ORDERS". It automatically maps incoming data fields like `orderID`, `customerID`, `employeeName`, `orderPrice`, and `orderStatus` to the corresponding columns in Airtable, ensuring data integrity and structured storage.

6.  **Code Node (Data Aggregation)**:
    *   **Purpose**: Performs custom calculations and data aggregation on the filtered and processed order items.
    *   **Configuration**: Executes a JavaScript snippet that iterates through all input items, calculating the `totalBooked` orders and the `bookedSum` (total value) of these orders. This node is critical for generating summarized reports.

7.  **Discord (Notification Service)**:
    *   **Purpose**: To send automated notifications about the workflow's outcomes to a Discord channel.
    *   **Configuration**: Utilizes a Discord webhook to post messages. The message content is dynamically generated to include the calculated `totalBooked` orders, their `bookedSum`, and the unique ID from the initial HTTP request, providing a comprehensive and traceable summary to the designated team.

## Technologies Used

*   **n8n**: The core workflow automation platform that orchestrates all steps.
*   **Airtable**: Used as a flexible and powerful database for managing and storing order records.
*   **Discord**: Leveraged for real-time notifications and team communication regarding workflow activities.
*   **HTTP/Webhooks**: Essential for integrating with external systems like the custom ERP.
*   **JavaScript**: Utilized within the Code node for custom data manipulation and aggregation.

## Setup & Deployment

To utilize or modify this workflow:

1.  **Import the Workflow**: Download the `NATHON WORKFLOW.json` file. In your n8n instance, click on "Workflows" in the sidebar, then "New", and choose "Import from JSON". Upload the `NATHON WORKFLOW.json` file.
2.  **Configure Credentials**: You will need to set up credentials for:
    *   **HTTP Header Authentication**: For the ERP system.
    *   **Airtable Personal Access Token**: To connect to your Airtable base.
    *   **Discord Webhook**: To send messages to your desired Discord channel.
    Follow the prompts within n8n's credential setup for each node requiring authentication.
3.  **Activate the Workflow**: Once credentials are configured and saved, activate the workflow within n8n. It will then run according to its schedule or can be triggered manually for testing.

## Contribution

Contributions to enhance this workflow, add new functionalities, or improve existing logic are welcome. Please refer to standard Git practices for contributing.

## License

This project is open-source. Please refer to the `LICENSE` file (if present) in the repository root for details on licensing. If no explicit license is provided, standard open-source conventions apply.

---