# Approvals and Notifications in Power Automate

Approval processes and notifications are crucial components of business workflows. Power Automate provides robust capabilities for implementing sophisticated approval systems and sending contextual notifications across multiple channels.

## Approval Flows

Approval flows enable you to create structured review and authorization processes for business decisions.

### Types of Approvals

Power Automate offers several approval types to match different business scenarios:

#### 1. Single-person Approval
- One designated person must approve or reject
- Best for straightforward decisions with clear ownership
- Example: Manager approval for time off requests

#### 2. Everyone Must Approve
- Multiple people must all approve for the request to proceed
- Used when complete consensus is required
- Example: Purchase requisitions requiring approval from multiple departments

#### 3. First to Respond
- Any designated approver can make the decision
- Used when you need a quick decision from any qualified person
- Example: Content review where any editor can approve

#### 4. Sequential Approval
- Approvers must review in a specific order
- Each approver only sees the request after previous approvals
- Example: Multi-level expense approvals (manager, then finance)

![Types of Approvals](images/approval-types.png)

### Setting Up Approval Flows

To create an approval workflow in Power Automate:

1. **Start with a Trigger**:
   - Could be a form submission, email, SharePoint item, or manual trigger
   - Captures the information needed for the approval decision

2. **Add an Approval Action**:
   - Search for "Approvals" in the actions list
   - Select the appropriate approval type
   - Configure required fields:
     - Title: Clear description of what's being approved
     - Assigned To: Email address(es) of approver(s)
     - Details: Information needed to make the decision
     - Item Link: Optional link to the item being approved
     - Attachments: Supporting documents

3. **Add Conditional Paths**:
   - Create separate paths for approved and rejected outcomes
   - Use "Condition" control with the outcome from the approval step

4. **Add Follow-up Actions**:
   - If approved: complete the business process
   - If rejected: notify the requester, update records, etc.

```
// Conceptual flow structure
Trigger: Form submission for expense
   |
   v
Start approval: Request expense approval
   |
   v
Condition: Outcome is 'Approve'?
   |
   |---Yes---> Update record as approved
   |            Send approval notification
   |            Process reimbursement
   |
   |---No----> Update record as rejected
                Send rejection notification with comments
```

![Approval Flow Example](images/approval-flow-example.png)

### Adaptive Cards for Approvals

Adaptive Cards provide rich, interactive approval experiences in Microsoft Teams, Outlook, and mobile apps.

#### Benefits of Adaptive Cards

- **Rich Visual Presentation**: Display important information in a structured format
- **Interactive Elements**: Buttons, input fields, and choice controls
- **Consistent Experience**: Same card works across multiple Microsoft platforms
- **Mobile-friendly**: Responsive design that works well on all devices
- **Branding Options**: Customize with company colors and logos

#### Creating Approval Cards

Power Automate automatically generates Adaptive Cards for approvals, but you can customize them:

1. **Standard Elements**:
   - Title and description
   - Approval buttons
   - Comment field for approver feedback

2. **Customization Options**:
   - Add company branding
   - Include relevant data fields
   - Attach supporting documents
   - Configure action button styling

```json
// Example of an Adaptive Card JSON structure (simplified)
{
    "type": "AdaptiveCard",
    "body": [
        {
            "type": "TextBlock",
            "size": "Medium",
            "weight": "Bolder",
            "text": "Expense Approval Request"
        },
        {
            "type": "FactSet",
            "facts": [
                { "title": "Submitted By:", "value": "John Doe" },
                { "title": "Amount:", "value": "$1,250.00" },
                { "title": "Category:", "value": "Travel" },
                { "title": "Date:", "value": "2023-10-15" }
            ]
        },
        {
            "type": "Input.Text",
            "id": "comment",
            "placeholder": "Add your comments here (optional)",
            "isMultiline": true
        }
    ],
    "actions": [
        {
            "type": "Action.Submit",
            "title": "Approve",
            "data": { "actionResponse": "approve" }
        },
        {
            "type": "Action.Submit",
            "title": "Reject",
            "data": { "actionResponse": "reject" }
        }
    ]
}
```

![Adaptive Card Example](images/adaptive-card-example.png)

### Managing Approval Responses

After sending an approval request, your flow must handle the response:

#### Waiting for Responses

- Use the "Wait for an approval" action after sending the request
- This pauses the flow until the approval decision is made
- Configure timeout settings for time-sensitive approvals

#### Handling Approval Decisions

1. **Accessing Response Data**:
   - Outcome: "Approve" or "Reject"
   - Comments: Feedback provided by the approver
   - Responder: Who made the decision
   - Response date: When the decision was made

2. **Conditional Processing**:
   - Create condition based on the "Outcome" property
   - Define different actions for "Approve" and "Reject" paths

3. **Response Metadata**:
   - Track approval metrics (response time, approval rates)
   - Store decision audit trail for compliance

#### Handling Multiple Approvers

For flows with multiple approvers:

- **Everyone must approve**: Check if all responses are "Approve"
- **First to respond**: Use the first response received
- **Sequential approvals**: Track each step and move to the next approver

```
// Example handling multiple approver responses
Apply to each: Responses
   |
   v
Condition: Outcome equals 'Reject'
   |
   |---Yes---> Increment variable: RejectCount
   |
   |---No----> Increment variable: ApproveCount
   |
   v
After the loop:
Condition: RejectCount greater than 0
   |
   |---Yes---> Process as rejected
   |
   |---No----> Process as approved
```

## Notifications in Power Automate

Notifications keep stakeholders informed about important events, status changes, and required actions.

### Notification Channels

Power Automate supports multiple notification channels:

#### Email Notifications

- **Outlook/Office 365**: Most common channel for business communications
- **Options**: HTML formatting, attachments, priority settings
- **Best for**: Formal communications, detailed information, users outside your organization

```
// Example email notification configuration
Send an email (V2):
   To: triggerBody()?['requesterEmail']
   Subject: concat('Your expense request for $', triggerBody()?['amount'], ' has been ', variables('approvalStatus'))
   Body: HTML with formatting and dynamic content
   Importance: Normal/High
   Attachments: Add relevant files
```

#### Microsoft Teams Notifications

- **Direct Messages**: Send to specific individuals
- **Channel Posts**: Share with entire teams or channels
- **Adaptive Cards**: Interactive cards with buttons and forms
- **Best for**: Internal communications, collaborative discussions, quick updates

```
// Example Teams notification
Post a message in a chat or channel:
   Post as: Flow bot
   Post in: Channel
   Team: Finance Team
   Channel: Approvals
   Message: Adaptive card or text message
```

#### Mobile Notifications

- **Push Notifications**: Alerts to mobile devices
- **SMS Texts**: Text messages to phone numbers
- **Best for**: Urgent notifications, field workers, time-sensitive alerts

```
// Example mobile notification
Send a mobile notification:
   User: Email of the user
   Message: Your approval is required
   Link to item needing action
```

#### Other Notification Channels

- **SharePoint Alerts**: Updates in SharePoint sites
- **Power Apps Notifications**: In-app notifications
- **Custom Webhooks**: Notifications to other systems
- **Microsoft Planner**: Task assignments

### Creating Effective Notifications

Best practices for notifications in Power Automate:

#### Content Guidelines

1. **Clear Subject Lines**: Indicate purpose and urgency in the subject
2. **Concise Content**: Provide essential information without overwhelming
3. **Action-Oriented**: Clearly state what action is needed (if any)
4. **Personalization**: Include recipient's name and relevant context
5. **Branding**: Use consistent visual identity and formatting

#### Technical Implementation

1. **Dynamic Content**: Use variables and expressions to personalize messages
2. **Conditional Sending**: Only notify when necessary (avoid notification fatigue)
3. **HTML Formatting**: Use tables, colors, and formatting for readability
4. **Mobile Optimization**: Ensure notifications display well on mobile devices
5. **Deep Links**: Include direct links to relevant items or actions

### Notification Templates

Create reusable notification templates for consistent communication:

1. **Approval Request Notifications**:
   - Clear request description
   - Important details (amount, date, etc.)
   - Deadline for response
   - Direct link to approve/reject

2. **Status Update Notifications**:
   - Current status indication
   - Progress tracking information
   - Next steps or estimated completion
   - Contact for questions

3. **Action Required Notifications**:
   - Specific action needed
   - Deadline information
   - Consequences of inaction
   - How to complete the action

4. **FYI Notifications**:
   - Brief summary of information
   - Relevance to recipient
   - Additional resources if needed
   - No action required statement

![Notification Templates](images/notification-templates.png)

## Advanced Approval Scenarios

### Delegation and Reassignment

Allow approvals to be delegated when approvers are unavailable:

1. **Out-of-Office Handling**:
   - Check approver's calendar/status
   - Automatically route to delegate
   - Track original and delegate approvers

2. **Manual Reassignment**:
   - Allow approvers to forward requests
   - Update approval tracking with new assignee
   - Notify original requester of reassignment

### Escalation Processes

Implement escalation for delayed approvals:

1. **Reminder Notifications**:
   - Send gentle reminders after set time periods
   - Increase urgency with each reminder
   - Include easy access to approval item

2. **Time-based Escalation**:
   - After threshold, escalate to alternate approver
   - Configure different thresholds by priority
   - Notify original approver of escalation

```
// Conceptual escalation logic
Start approval request
   |
   v
Wait: 24 hours
   |
   v
Condition: Response received?
   |
   |---Yes---> Process response normally
   |
   |---No----> Send reminder to approver
   |            |
   |            v
   |          Wait: 24 more hours
   |            |
   |            v
   |          Condition: Response received?
   |            |
   |            |---Yes---> Process response
   |            |
   |            |---No----> Escalate to manager
```

### Multi-stage Approvals

Implement complex approval processes with multiple stages:

1. **Sequential Stages**:
   - Line manager → Department head → Finance
   - Each approval unlocks the next stage
   - Previous approvers can see subsequent decisions

2. **Conditional Stages**:
   - Different approval paths based on criteria
   - Example: High-value purchases require additional approvals
   - Skip unnecessary stages when possible

3. **Parallel with Dependencies**:
   - Some approvals happen simultaneously
   - Others require prerequisites
   - Track complex dependencies between approvers

### Integration with Business Systems

Connect approval processes with business systems:

1. **ERP Integration**:
   - Update purchase orders after approval
   - Create invoices automatically
   - Sync approval status with financial systems

2. **HR System Integration**:
   - Update time off balances
   - Record approved changes to employee records
   - Trigger payroll adjustments

3. **Document Management**:
   - Update document metadata with approval status
   - Apply retention policies based on approval
   - Generate audit trails for compliance

## Best Practices for Approvals and Notifications

### Approval Best Practices

1. **Clear Context**: Provide approvers with all necessary information
2. **Mobile-Friendly**: Ensure approvals work well on mobile devices
3. **Reasonable Deadlines**: Set appropriate timeframes for decisions
4. **Transparent Process**: Keep all stakeholders informed of status
5. **Audit Trail**: Maintain complete records of approval decisions

### Notification Best Practices

1. **Targeted Communication**: Only notify relevant stakeholders
2. **Notification Timing**: Send at appropriate times (avoid after hours)
3. **Balanced Frequency**: Avoid notification fatigue
4. **Actionable Format**: Make it clear what action is needed
5. **Consistent Experience**: Use standard templates and formatting

### Governance Considerations

1. **Compliance Requirements**:
   - Ensure approvals meet regulatory standards
   - Maintain records for required periods
   - Implement appropriate security controls

2. **Process Documentation**:
   - Document approval workflows clearly
   - Include escalation paths and exceptions
   - Train users on the process

3. **Monitoring and Analytics**:
   - Track approval metrics (time to approve, approval rates)
   - Identify bottlenecks in processes
   - Continuously improve based on data

In the next section, we'll explore how Power Automate integrates with other Microsoft products and services. 