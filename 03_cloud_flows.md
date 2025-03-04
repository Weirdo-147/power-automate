# Cloud Flows in Power Automate

Cloud flows are the backbone of Power Automate, enabling you to connect cloud services and automate tasks without worrying about infrastructure. These flows run entirely in the Microsoft cloud and can connect to hundreds of services and data sources.

## Types of Cloud Flows

Power Automate offers three primary types of cloud flows, each designed for different automation scenarios:

### 1. Automated Flows

Automated flows are triggered by specific events in connected services. They run automatically when the specified trigger event occurs.

#### Key Characteristics:

- **Event-driven**: Starts automatically when a specific event happens
- **Unattended**: Runs without user intervention
- **System-connected**: Triggered by system events and changes

#### Common Triggers for Automated Flows:

- When an email arrives with an attachment
- When a file is created or modified in SharePoint
- When a new row is added to a database
- When a form is submitted
- When a tweet mentions your company
- When a new lead is created in Dynamics 365

#### Example: Document Processing Flow

```
Trigger: When a new file is added to SharePoint
   |
   v
Action: Get file content
   |
   v
Condition: Is file type PDF?
   |
   |---Yes---> Action: Extract text from PDF using AI Builder
   |             |
   |             v
   |           Action: Process extracted text
   |
   |---No----> Action: Send notification about invalid file type
```

![Automated Flow Example](images/automated-flow-example.png)

### 2. Instant Flows

Instant flows are manually triggered by users and are perfect for on-demand automation tasks.

#### Key Characteristics:

- **User-initiated**: Started manually by a user
- **Interactive**: Can collect inputs from users
- **Contextual**: Can be triggered from various contexts (mobile, web, Teams)

#### Ways to Trigger Instant Flows:

- From the Power Automate mobile app
- From a button in Microsoft Teams
- From a button in SharePoint
- From the Power Automate web portal
- From a Power Apps application
- As a context menu option in Dynamics 365

#### Example: Expense Approval Flow

```
Trigger: Manual button click
   |
   v
Action: Get user input (expense amount, category, receipt)
   |
   v
Action: Create expense record in database
   |
   v
Action: Start approval process with manager
   |
   v
Condition: Approved?
   |
   |---Yes---> Action: Record approval and notify user
   |
   |---No----> Action: Send rejection notification with comments
```

![Instant Flow Example](images/instant-flow-example.png)

### 3. Scheduled Flows

Scheduled flows run at specified times and intervals, making them ideal for regular, recurring tasks.

#### Key Characteristics:

- **Time-based**: Runs according to a schedule
- **Recurring**: Can repeat at defined intervals
- **Predictable**: Executes at the same time regardless of system events

#### Scheduling Options:

- **Frequency**: Set to minute, hour, day, week, or month
- **Interval**: Specify how many units of the frequency to wait
- **Start time**: Define when the schedule should begin
- **Time zone**: Set the time zone for the schedule
- **On specific days**: For weekly or monthly, specify which days

#### Example: Weekly Report Flow

```
Trigger: Schedule (Weekly on Monday at 9:00 AM)
   |
   v
Action: Query database for weekly metrics
   |
   v
Action: Format data into report
   |
   v
Action: Create Excel file with report data
   |
   v
Action: Send email with report attachment to stakeholders
```

![Scheduled Flow Example](images/scheduled-flow-example.png)

## Building Effective Cloud Flows

### Design Principles

1. **Start Simple**: Begin with a clear, focused flow that accomplishes a single task
2. **Build Incrementally**: Test each step as you add it to ensure it works as expected
3. **Use Templates**: Leverage pre-built templates to accelerate development
4. **Follow Naming Conventions**: Use descriptive names for flows and actions
5. **Document Flows**: Add comments and descriptions for complex logic

### Best Practices

- **Error Handling**: Add error handling to make flows resilient
- **Parallelism**: Use "Apply to each" for processing multiple items simultaneously
- **Throttling Awareness**: Be mindful of connector limitations and rate limits
- **Connection References**: Use connection references in solutions for better governance
- **Testing**: Thoroughly test flows with varied inputs before deployment

## Advanced Cloud Flow Features

### Flow Approvals

Power Automate includes built-in approval processes that can be added to any flow:

- **Approval Types**: Single approver, everyone must approve, first to respond
- **Approval Interfaces**: Email, Teams, Power Apps, SharePoint
- **Custom Approval Emails**: Design personalized approval request emails
- **Escalation**: Create escalation paths for delayed approvals

### Flow Connections

Connections are secure ways to connect your flow to different services:

- **Connection Authentication**: OAuth, API Key, Basic Auth, and more
- **Connection Management**: Centralized connection management in the Power Platform admin center
- **Shared Connections**: Ability to share connections with colleagues
- **On-premises Data Gateway**: Connect to on-premises data sources

### Flow Expressions

Expressions allow you to perform operations on data within your flow:

- **Data Manipulation**: transform, format, and parse data
- **Date/Time Functions**: calculate dates and format timestamps
- **Logic Functions**: implement complex conditional logic
- **Collection Operations**: work with arrays and objects

```
// Example expressions
concat('Hello', ' ', triggerBody()?['name']) // String concatenation
formatDateTime(utcNow(), 'yyyy-MM-dd') // Date formatting
if(equals(variables('status'), 'approved'), 'Proceed', 'Stop') // Conditional logic
```

## Flow Management and Governance

### Organizing Flows

- **Solutions**: Group related flows in solutions for better management
- **Environments**: Separate development, test, and production environments
- **Flow Ownership**: Transfer ownership when team members change

### Monitoring and Analytics

- **Run History**: View detailed execution history for debugging
- **Analytics**: Track success rates and performance metrics
- **Alerts**: Set up alerts for flow failures

### Security and Compliance

- **Data Loss Prevention (DLP)**: Apply DLP policies to protect sensitive data
- **Access Control**: Manage who can run and edit flows
- **Audit Logs**: Track flow creation and modification

In the next section, we'll explore the extensive library of triggers and connectors available in Power Automate. 