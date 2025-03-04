# Triggers and Connectors in Power Automate

Triggers and connectors form the foundation of Power Automate, enabling your flows to integrate with hundreds of services and respond to events across your digital ecosystem.

## Understanding Triggers

Triggers are events that start your flow. Every flow must begin with a trigger, which defines when and why the flow should run.

### Types of Triggers

#### Event-based Triggers
These triggers activate when a specific event occurs in a connected service:
- When an email arrives in Outlook
- When a file is modified in SharePoint
- When a new lead is created in Dynamics 365
- When a form is submitted

#### Polling Triggers
These triggers check for changes at regular intervals:
- Check for new tweets with specific hashtags
- Monitor RSS feeds for new items
- Check for new rows in a database

#### Manual Triggers
These triggers are activated by a user:
- Button clicks
- Power Apps form submissions
- Mobile app actions
- Teams commands

#### Scheduled Triggers
These triggers activate based on a predefined schedule:
- Run daily at 8 AM
- Run every Monday at 9 AM
- Run every hour

### Common Trigger Properties

Most triggers share these common configuration options:

- **Frequency**: How often to check for the triggering condition
- **Concurrency Control**: Limit the number of concurrent runs
- **Split On**: Process trigger outputs as separate flows
- **Trigger Conditions**: Additional conditions that must be met
- **SplitBatch**: Process batches of trigger outputs

![Trigger Configuration](images/trigger-configuration.png)

## Understanding Connectors

Connectors are the bridges between Power Automate and the services you want to automate. Each connector provides a way for your flows to access another service or application.

### Connector Categories

#### Standard Connectors
Included with all Power Automate licenses:
- Microsoft 365 (Outlook, Teams, SharePoint, OneDrive)
- Common data services (SQL, Excel, CSV)
- Social media (Twitter, Facebook)
- Basic productivity tools (Notifications, Approvals)

#### Premium Connectors
Require Power Automate premium plans:
- Advanced Microsoft services (Dynamics 365, Power Apps)
- Enterprise systems (SAP, Oracle)
- Third-party business applications (Salesforce, ServiceNow, Workday)
- Enhanced services (DocuSign, Adobe)

#### Custom Connectors
Created to connect to your own services:
- Connect to REST APIs
- Connect to SOAP web services
- Connect to Azure Functions
- Connect to on-premises systems

### Authentication and Connections

Connectors use connections to authenticate with their respective services:

- **OAuth**: Used for Microsoft services and many third-party services
- **API Key**: Used for simple API authentication
- **Username/Password**: Used for basic authentication
- **Certificate**: Used for secure enterprise connections
- **Gateway**: Used for on-premises data connections

Connections are created and managed separately from flows, allowing them to be reused across multiple flows.

## Built-in Connectors

Power Automate includes hundreds of built-in connectors. Here are some of the most popular ones:

### Microsoft 365 Connectors

#### Outlook
- Triggers: New email, Calendar event created
- Actions: Send email, Create event, Create contact

#### SharePoint
- Triggers: File created/modified, Item created/modified
- Actions: Create file, Update item, Get list items

#### Teams
- Triggers: New message, New channel
- Actions: Post message, Create channel, Get team details

#### OneDrive
- Triggers: File created/modified
- Actions: Create file, List folder contents, Copy file

### Common Data Services

#### Dataverse
- Triggers: Record created/updated/deleted
- Actions: Create/update/delete records, Retrieve multiple rows

#### SQL Server
- Triggers: When a row is added/modified
- Actions: Insert row, Get rows, Execute query

#### Excel Online
- Actions: Add row, List rows, Update row

### Other Popular Connectors

#### Power Apps
- Trigger: When a Power Apps app is run
- Actions: Respond to Power Apps

#### Power BI
- Triggers: Data alert triggered
- Actions: Add rows to dataset, Refresh dataset

#### Azure Services
- Azure Blob Storage, Azure Functions, Event Grid, Logic Apps

![Popular Connectors](images/popular-connectors.png)

## Using Triggers in Flows

### Configuring Triggers

When setting up a trigger, you typically need to:

1. **Select the Connection**: Choose which account/connection to use
2. **Provide Required Parameters**: For example, which folder to monitor in SharePoint
3. **Set Trigger Conditions**: Define additional filtering criteria
4. **Configure Advanced Options**: Set polling frequency or concurrency settings

### Trigger Best Practices

- **Choose Specific Triggers**: Use specific triggers (e.g., "When an email arrives from specific sender") rather than general ones
- **Use Trigger Conditions**: Filter at the trigger level to improve performance
- **Consider Polling Intervals**: Balance responsiveness with resource utilization
- **Implement Error Handling**: Add retry policies for intermittent failures
- **Test Thoroughly**: Ensure your trigger responds correctly to different event scenarios

## Using Connectors in Flows

### Adding Connector Actions

To use a connector in your flow:

1. Click "+ New step" in the flow designer
2. Search for the connector you want to use
3. Select the specific action from that connector
4. Configure the action with required parameters
5. Use dynamic content from previous steps as needed

### Connector Limitations

Be aware of these common connector limitations:

- **API Throttling**: Many services limit the number of API calls
- **Timeout Limits**: Actions must complete within timeout limits
- **Input/Output Size**: There are limits on data sizes
- **Authentication Expiration**: Connections may need periodic renewal
- **Regional Availability**: Some connectors may not be available in all regions

```
// Example of connector usage in expressions
// Get the email of the person who modified a SharePoint file
outputs('Get_file_properties')?['LastModifiedBy']?['Email']

// Format a date from a SQL query result
formatDateTime(items('Apply_to_each')?['Created_Date'], 'yyyy-MM-dd')
```

## Custom Connectors

When built-in connectors don't meet your needs, you can create custom connectors.

### When to Use Custom Connectors

- Connecting to internal company APIs
- Using specialized third-party services without built-in connectors
- Extending existing connectors with custom functionality
- Simplifying complex API interactions for business users

### Creating a Custom Connector

1. **Define the API**: Use an OpenAPI (Swagger) definition or create one manually
2. **Configure Authentication**: Set up the authentication type (OAuth, API Key, etc.)
3. **Define Actions and Triggers**: Specify the operations available in your connector
4. **Configure Request/Response**: Define data models for requests and responses
5. **Test the Connector**: Validate each operation works as expected
6. **Share the Connector**: Make it available to other flow creators in your organization

![Custom Connector Creation](images/custom-connector-creation.png)

## On-premises Data Gateway

The on-premises data gateway allows Power Automate to securely connect to on-premises data sources.

### Gateway Features

- **Secure Connections**: Encrypted connections to on-premises resources
- **Multiple Data Sources**: Connect to various on-premises systems
- **High Availability**: Set up clusters for reliability
- **Monitoring**: Monitor gateway performance and usage
- **Central Management**: Manage gateways through the Power Platform admin center

### Supported Data Sources

- SQL Server
- SharePoint Server
- Oracle Database
- File shares
- IBM DB2
- SAP systems (with premium connectors)

### Setting Up the Gateway

1. Download and install the on-premises data gateway
2. Register the gateway with your Power Platform account
3. Create connections to on-premises data sources
4. Use these connections in your flows

## Advanced Connector Scenarios

### Hybrid Connectivity

Combine cloud and on-premises systems in the same flow:
- Trigger from Dynamics 365 Online
- Query on-premises SQL Server
- Update SharePoint Online
- Send notification via Teams

### Multi-connection Scenarios

Use multiple connections of the same type for cross-system operations:
- Copy files between two SharePoint sites
- Compare data between two SQL databases
- Transfer emails between Outlook accounts

### Error Handling with Connectors

Implement robust error handling:
- Use retry policies for transient failures
- Implement try-catch patterns with "Run after" configurations
- Create notification pathways for connection failures
- Log detailed error information for troubleshooting

In the next section, we'll explore how to use actions and conditions to create sophisticated flow logic. 