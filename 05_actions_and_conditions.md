# Actions and Conditions in Power Automate

Actions and conditions are the building blocks that determine what your flow does and how it makes decisions. They allow you to create complex business logic without coding expertise.

## Understanding Actions

Actions are the tasks that your flow performs. Each action is a discrete operation that interacts with a connector or performs a function within Power Automate.

### Types of Actions

#### Connector Actions
These actions interact with specific services through connectors:
- Send an email (Outlook)
- Create a file (SharePoint)
- Post a message (Teams)
- Update a record (Dynamics 365)

#### Built-in Actions
These actions are part of Power Automate's core functionality:
- Compose (create or transform data)
- Variables (create, set, increment variables)
- HTTP (make direct API calls)
- Parse JSON (work with JSON data)

#### Control Actions
These actions control the flow's execution path:
- Condition (if-then-else logic)
- Switch (multiple condition paths)
- Apply to each (loop through arrays)
- Do until (repeat until condition is met)
- Scope (group actions together)

![Action Types](images/action-types.png)

### Common Action Properties

Most actions share these configuration options:

- **Inputs**: The data required by the action to perform its task
- **Dynamic Content**: Values from previous steps that can be used as inputs
- **Settings**: Action-specific configuration options
- **Run After**: Configure when the action should run (success, failure, skipped)
- **Retry Policy**: Configure automatic retries for failed actions

## Working with Control Actions

Control actions add logic and structure to your flows, allowing you to handle complex scenarios.

### Condition

The Condition action creates a branch in your flow based on a true/false evaluation.

#### How to Use Conditions:

1. Add a "Condition" control action to your flow
2. Configure the condition with:
   - Left value (often dynamic content from a previous step)
   - Operator (equals, greater than, less than, contains, etc.)
   - Right value (what you're comparing against)
3. Add actions to the "If yes" and "If no" branches

#### Example: Expense Approval Based on Amount

```
Condition: Is Expense Amount greater than 1000?
   |
   |---Yes---> Action: Send approval request to director
   |            Action: Wait for approval response
   |
   |---No----> Action: Auto-approve
               Action: Send confirmation to requester
```

![Condition Example](images/condition-example.png)

### Switch

The Switch action creates multiple branches based on the value of an expression.

#### How to Use Switch:

1. Add a "Switch" control action to your flow
2. Define the expression to evaluate (the "On" value)
3. Add cases for different possible values
4. Add a default case for values not explicitly handled

#### Example: Process Different File Types

```
Switch: On File Extension
   |
   |---Case: .pdf ---> Action: Process with PDF parser
   |
   |---Case: .docx --> Action: Process with Word extractor
   |
   |---Case: .xlsx --> Action: Process with Excel actions
   |
   |---Default ------> Action: Send "unsupported format" notification
```

![Switch Example](images/switch-example.png)

### Apply to Each

The Apply to Each action processes each item in an array or collection.

#### How to Use Apply to Each:

1. Add an "Apply to each" control action to your flow
2. Select an array to iterate through (often from a previous step)
3. Add actions inside the loop that will process each item
4. Use the "Current item" dynamic value to reference the item being processed

#### Example: Process Attachments in an Email

```
Trigger: When a new email arrives with attachments
   |
   v
Get attachment metadata
   |
   v
Apply to each: Attachments array
   |
   |---> Action: Download attachment content
   |
   |---> Action: Save to SharePoint
   |
   |---> Condition: Is file size > 10MB?
   |      |
   |      |---Yes---> Action: Compress file
   |      |
   |      |---No----> Action: Skip compression
```

![Apply to Each Example](images/apply-to-each-example.png)

### Do Until

The Do Until action repeats a set of actions until a condition is met.

#### How to Use Do Until:

1. Add a "Do until" control action to your flow
2. Configure the condition that will end the loop
3. Add actions inside the loop
4. Set a limit for the number of iterations and timeout duration

#### Example: Polling for Status Change

```
Do Until: Status equals "Completed"
   |
   |---> Action: Get current status from API
   |
   |---> Action: Wait for 5 minutes
   |
   (Loop continues until status is "Completed" or limit reached)
```

![Do Until Example](images/do-until-example.png)

### Scope

The Scope action groups other actions together for organization and error handling.

#### How to Use Scope:

1. Add a "Scope" control action to your flow
2. Add related actions inside the scope
3. Configure error handling for the entire scope
4. Use scopes to organize complex flows

#### Example: Transaction Processing with Error Handling

```
Scope: Process Transaction
   |
   |---> Action: Begin Transaction
   |
   |---> Action: Update Inventory
   |
   |---> Action: Process Payment
   |
   |---> Action: Send Receipt

(If any action fails, run the "Transaction Failed" scope)

Scope: Transaction Failed
   |
   |---> Action: Rollback Transaction
   |
   |---> Action: Log Error
   |
   |---> Action: Notify Support
```

![Scope Example](images/scope-example.png)

## Data Operations Actions

Data Operations actions help you manipulate and transform data within your flows.

### Compose

The Compose action creates a new value that can be used in later steps.

```
// Example: Compose a greeting message
Compose: concat('Hello, ', triggerBody()?['name'], '! Today is ', formatDateTime(utcNow(), 'dddd'))
```

### Variables

Variables let you store and manipulate values throughout your flow.

- **Initialize variable**: Create a variable of type string, integer, boolean, array, or object
- **Set variable**: Change the value of an existing variable
- **Increment variable**: Increase a numeric variable
- **Append to array variable**: Add items to an array variable

```
// Example: Track processed items
Initialize variable: Count (integer) = 0
Apply to each: Items array
   |
   |---> Process item actions...
   |
   |---> Increment variable: Count by 1
```

### Parse JSON

The Parse JSON action converts JSON strings into structured data that can be referenced in later steps.

```
// Example: Parsing a JSON response from an API
Parse JSON:
   Content: @body('HTTP_Request')
   Schema: {
     "type": "object",
     "properties": {
       "id": { "type": "string" },
       "name": { "type": "string" },
       "status": { "type": "string" }
     }
   }
```

### Select

The Select action transforms an array by projecting each item into a new format.

```
// Example: Extracting specific fields from an array of records
Select:
   From: @outputs('Get_items')?['body/value']
   Map: {
     "EmployeeID": @item()?['id'],
     "FullName": @{concat(item()?['firstName'], ' ', item()?['lastName'])},
     "Department": @item()?['department/name']
   }
```

### Filter Array

The Filter Array action creates a subset of an array based on a condition.

```
// Example: Filtering for high-priority items
Filter Array:
   From: @outputs('Get_items')?['body/value']
   Where: @equals(item()?['priority'], 'High')
```

## Working with Expressions

Expressions allow you to perform calculations, transformations, and evaluations within your flows.

### Expression Basics

Expressions in Power Automate use a syntax similar to JavaScript and are enclosed in `@{ }` when used in text fields:

```
@{expression}
```

When used in dedicated expression fields, you omit the `@{ }`:

```
expression
```

### Common Expression Functions

#### String Functions

- **concat()**: Combine strings
  ```
  concat('Hello', ' ', 'World') // Result: "Hello World"
  ```

- **replace()**: Replace text in a string
  ```
  replace('Hello World', 'World', 'Universe') // Result: "Hello Universe"
  ```

- **substring()**: Extract part of a string
  ```
  substring('Hello World', 0, 5) // Result: "Hello"
  ```

#### Date/Time Functions

- **utcNow()**: Get the current UTC date and time
  ```
  utcNow() // Result: "2023-09-15T14:30:00.0000000Z"
  ```

- **formatDateTime()**: Format a date/time value
  ```
  formatDateTime(utcNow(), 'yyyy-MM-dd') // Result: "2023-09-15"
  ```

- **addDays()**: Add days to a date
  ```
  addDays(utcNow(), 7) // Result: Date 7 days from now
  ```

#### Logical Functions

- **if()**: Conditional evaluation
  ```
  if(equals(variables('status'), 'approved'), 'Proceed', 'Stop')
  ```

- **and(), or(), not()**: Logical operators
  ```
  and(greater(variables('count'), 5), less(variables('count'), 10))
  ```

#### Collection Functions

- **length()**: Get the size of an array
  ```
  length(variables('myArray'))
  ```

- **first(), last()**: Get the first or last item of an array
  ```
  first(outputs('Get_items')?['body/value'])
  ```

- **union()**: Combine arrays
  ```
  union(variables('array1'), variables('array2'))
  ```

## Advanced Action Techniques

### Action Dependencies and Run After

By default, actions run sequentially. You can modify this behavior using the "Run after" configuration:

- **Run after success**: The default behavior
- **Run after failure**: Run only if the previous action fails
- **Run after skipped**: Run only if the previous action is skipped
- **Run after timeout**: Run if the previous action times out

This enables complex error handling and alternate paths in your flow.

### Concurrent Actions

To improve performance, you can configure actions to run in parallel by:

1. Adding multiple actions after the same step
2. Setting each to run after the same previous step
3. This allows independent actions to execute simultaneously rather than sequentially

### Error Handling

Implement robust error handling with these techniques:

1. **Configure Run After**: Set actions to run after failures
2. **Use Scopes**: Group actions and handle errors at the scope level
3. **Try-Catch Pattern**: Create parallel paths for success and failure scenarios
4. **Terminate Action**: End a flow with success or failure status and a message

### Timeouts and Retries

For improved reliability:

1. **Action Timeouts**: Configure how long to wait for an action to complete
2. **Retry Policies**: Set automatic retries for failed actions:
   - **Default**: No retries
   - **Fixed Interval**: Retry at regular intervals
   - **Exponential Interval**: Increasing wait time between retries

## Best Practices for Actions and Conditions

### Design Best Practices

1. **Keep It Simple**: Break complex flows into multiple smaller flows
2. **Use Clear Names**: Give actions descriptive names for easier troubleshooting
3. **Organize with Scopes**: Group related actions in scopes
4. **Document Complex Logic**: Add comments to explain complex conditions
5. **Validate Inputs**: Check that required data exists before using it

### Performance Best Practices

1. **Limit Apply to Each Nesting**: Avoid deeply nested loops
2. **Use Filter Before Apply to Each**: Reduce the number of items to process
3. **Batch Operations**: Use batch actions when available
4. **Selective Retrieval**: Only request the data you need from APIs
5. **Debounce Frequent Triggers**: Use conditions to limit processing of rapid events

### Debugging Tips

1. **Add Compose Actions**: Use Compose to view intermediate values
2. **Track with Variables**: Use variables to track progress and state
3. **Implement Logging**: Add steps to log key information to a database or file
4. **Test with Sample Data**: Use the test feature with various inputs
5. **Inspect Run History**: Review detailed execution history for troubleshooting

In the next section, we'll explore how to implement approvals and notifications in your flows. 