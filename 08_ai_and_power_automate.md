# AI and Power Automate

Artificial Intelligence capabilities in Power Automate enable you to incorporate intelligent processing into your automation workflows without requiring data science expertise. Through AI Builder integration, Power Automate offers pre-built AI models and the ability to create custom models for specific business scenarios.

## Using AI Builder in Power Automate

AI Builder is a low-code AI capability that empowers you to leverage the power of artificial intelligence in your flows.

### AI Builder Integration Overview

AI Builder connects directly with Power Automate, providing:

- **Pre-built AI Models**: Ready-to-use models for common scenarios
- **Custom Model Creation**: Build your own AI models for specific needs
- **Simple Implementation**: Add AI capabilities through standard flow actions
- **Seamless Integration**: Use AI insights directly in your flows
- **Low-code Experience**: No data science or programming expertise required

![AI Builder Overview](images/ai-builder-overview.png)

### Types of AI Models

AI Builder offers several types of models that can be used in Power Automate:

#### Pre-built Models

These models are ready to use without training:

- **Text Recognition (OCR)**: Extract text from images and documents
- **Business Card Reader**: Extract contact information from business cards
- **ID Reader**: Extract information from identity documents
- **Receipt Processing**: Extract line items and totals from receipts
- **Language Detection**: Identify the language of text
- **Key Phrase Extraction**: Identify key phrases in text
- **Sentiment Analysis**: Determine sentiment (positive/negative) of text
- **Text Translation**: Translate text between languages
- **Entity Extraction**: Identify entities like people, places, organizations
- **Text Moderation**: Detect inappropriate content

#### Custom Models

These models require training with your own data:

- **Form Processing**: Extract specific information from your forms
- **Object Detection**: Identify and locate objects in images
- **Text Classification**: Categorize text into custom categories
- **Category Classification**: Classify items into categories
- **Prediction**: Predict outcomes based on historical data

### Adding AI to Your Flows

Incorporating AI Builder capabilities into Power Automate flows is straightforward:

1. **Add AI Builder Action**: Search for "AI Builder" in the actions list
2. **Select the Model Type**: Choose from available pre-built or custom models
3. **Configure the Input**: Provide the data to be processed (text, image, etc.)
4. **Process the Results**: Use the AI outputs in subsequent flow steps

```
// Conceptual flow structure for AI integration
Trigger: When a document is uploaded to SharePoint
   |
   v
AI Builder: Extract text from image
   |
   v
Parse extracted text (using expressions)
   |
   v
Condition: Contains sensitive information?
   |
   |---Yes---> Start approval process
   |            Apply security classification
   |
   |---No----> Process document normally
               Update metadata
```

![AI Builder Flow Integration](images/ai-builder-flow-integration.png)

## Text Recognition and Document Processing

AI-powered document processing can transform how you handle forms, receipts, IDs, and other paper-based information.

### Optical Character Recognition (OCR)

Extract text from images and documents:

#### Key Capabilities

- **General Text Extraction**: Read printed and handwritten text from images
- **Layout Preservation**: Understand document structure and layout
- **Multi-language Support**: Process text in various languages
- **Image Pre-processing**: Automatically enhance images for better results

#### Common Flow Scenarios

- **Document Digitization**: Convert paper documents to digital text
- **Business Card Processing**: Extract contact information to CRM
- **Image-based Search**: Enable searching through image content
- **Compliance Checking**: Verify document content against requirements

```
// Example: Extract text from uploaded images
Trigger: When an image is uploaded to SharePoint
   |
   v
AI Builder: Read text from image
   |
   v
Parse JSON: Structure the extracted text
   |
   v
Create SharePoint item with extracted text
   |
   v
Send notification with extraction summary
```

### Form Processing

Extract specific information from structured forms:

#### Key Capabilities

- **Field Identification**: Recognize named fields and values
- **Custom Form Training**: Train models on your specific form types
- **Table Extraction**: Process tabular data in forms
- **Confidence Scores**: Assess extraction reliability

#### Common Flow Scenarios

- **Automated Form Entry**: Extract data from forms to business systems
- **Invoice Processing**: Capture vendor, amounts, and line items
- **Application Processing**: Extract applicant information
- **Contract Analysis**: Extract key terms and dates

```
// Example: Process invoices
Trigger: When an invoice is received in shared mailbox
   |
   v
Get email attachment
   |
   v
AI Builder: Extract information from invoice
   |
   v
Create invoice record in accounting system
   |
   v
Condition: Requires approval? (based on amount)
   |
   |---Yes---> Start approval process
   |
   |---No----> Mark for payment
```

### Receipt and ID Processing

Specialized extraction for common document types:

#### Receipt Processing

- Extract merchant information, date, total amount
- Identify individual line items
- Categorize expenses
- Match against expense policies

#### ID Document Processing

- Extract name, address, ID number
- Verify document authenticity
- Capture photo from ID
- Support multiple ID types (driver's license, passport, etc.)

![Document Processing Examples](images/document-processing-examples.png)

## Sentiment Analysis and Text Analytics

Understand the meaning and sentiment behind text content.

### Sentiment Analysis

Determine the emotional tone of text:

#### Key Capabilities

- **Sentiment Scoring**: Rate text from negative to positive
- **Multi-language Support**: Analyze text in various languages
- **Mixed Sentiment Detection**: Identify multiple sentiments in longer text
- **Context Awareness**: Consider context for improved accuracy

#### Common Flow Scenarios

- **Customer Feedback Analysis**: Process survey responses
- **Social Media Monitoring**: Track brand sentiment
- **Email Prioritization**: Flag urgent or negative messages
- **Support Ticket Routing**: Direct based on customer sentiment

```
// Example: Route customer feedback based on sentiment
Trigger: When feedback form is submitted
   |
   v
AI Builder: Analyze sentiment of feedback text
   |
   v
Condition: Sentiment is negative?
   |
   |---Yes---> Create high-priority case
   |            Alert customer service manager
   |            Send immediate response to customer
   |
   |---No----> Log feedback in database
                Send standard thank you email
```

### Key Phrase Extraction

Identify the main points in text:

#### Key Capabilities

- **Topic Identification**: Extract key subjects discussed
- **Summary Generation**: Create text summaries
- **Important Point Extraction**: Identify critical information
- **Document Indexing**: Create searchable indices

#### Common Flow Scenarios

- **Document Tagging**: Automatically tag with key phrases
- **Content Summarization**: Generate executive summaries
- **Knowledge Base Indexing**: Improve searchability
- **Topic Tracking**: Monitor discussions around specific topics

### Language Detection and Translation

Process multilingual content:

#### Language Detection

- Identify the language of text content
- Support for 100+ languages
- Confidence scoring
- Multi-language document processing

#### Text Translation

- Translate between 100+ languages
- Preserve formatting where possible
- Domain-specific translations
- Batch translation capabilities

```
// Example: Translate and route customer inquiries
Trigger: When email arrives in support mailbox
   |
   v
AI Builder: Detect language of email
   |
   v
Condition: Language is not English?
   |
   |---Yes---> AI Builder: Translate to English
   |            Store original and translated versions
   |            Route to appropriate agent with translation
   |
   |---No----> Route directly to available agent
```

## Object Detection and Image Analysis

Process and analyze image content in your flows.

### Object Detection

Identify and locate objects within images:

#### Key Capabilities

- **Object Recognition**: Identify specific objects in images
- **Location Information**: Determine where objects appear in images
- **Multiple Object Detection**: Find various objects in a single image
- **Counting**: Count instances of specific objects

#### Common Flow Scenarios

- **Inventory Management**: Count items in images
- **Safety Compliance**: Detect safety equipment usage
- **Quality Control**: Identify defects or issues
- **Retail Shelf Analysis**: Monitor product placement

```
// Example: Construction site safety monitoring
Trigger: When site photo is uploaded to SharePoint
   |
   v
AI Builder: Detect safety equipment in image
   |
   v
Condition: Required safety equipment detected?
   |
   |---No----> Create safety incident report
   |            Send alert to safety officer
   |            Log violation with image evidence
   |
   |---Yes---> Log compliant site check
                Update safety compliance dashboard
```

### Custom Image Classification

Train models to classify images into your specific categories:

#### Key Capabilities

- **Custom Categories**: Define your own image classes
- **Visual Pattern Recognition**: Recognize visual patterns
- **Product Identification**: Classify products or items
- **Damage Assessment**: Identify damaged items

#### Common Flow Scenarios

- **Product Categorization**: Automatically tag product images
- **Damage Reporting**: Classify damage severity in insurance claims
- **Brand Compliance**: Verify visual branding elements
- **Content Moderation**: Filter inappropriate images

![Object Detection Example](images/object-detection-example.png)

## Prediction and Classification

Leverage machine learning to predict outcomes and classify data.

### Prediction Models

Forecast outcomes based on historical data:

#### Key Capabilities

- **Numeric Predictions**: Forecast values (sales, quantities, etc.)
- **Binary Predictions**: Yes/no outcome forecasting
- **Multiple-factor Analysis**: Consider multiple variables
- **Confidence Scoring**: Understand prediction reliability

#### Common Flow Scenarios

- **Sales Forecasting**: Predict future sales
- **Churn Prediction**: Identify at-risk customers
- **Resource Planning**: Forecast resource needs
- **Risk Assessment**: Evaluate potential risks

```
// Example: Proactive customer retention
Trigger: Schedule (Weekly customer analysis)
   |
   v
Get customer activity data
   |
   v
AI Builder: Predict customer churn likelihood
   |
   v
Filter array: Customers with high churn risk
   |
   v
Apply to each: High-risk customer
   |
   v
Create targeted retention offer
   |
   v
Send personalized outreach
```

### Text Classification

Categorize text into custom categories:

#### Key Capabilities

- **Custom Categories**: Define your own text classes
- **Multi-label Classification**: Assign multiple categories
- **Contextual Understanding**: Consider text context
- **Adaptive Learning**: Improve with more data

#### Common Flow Scenarios

- **Support Ticket Categorization**: Route to correct department
- **Content Tagging**: Categorize articles and documents
- **Email Routing**: Direct emails to appropriate teams
- **Feedback Classification**: Organize customer feedback

### Category Classification

Classify records based on multiple attributes:

#### Key Capabilities

- **Multi-attribute Analysis**: Consider multiple fields
- **Pattern Recognition**: Identify patterns in data
- **Probability Scoring**: Provide confidence levels
- **Explainable Results**: Understand classification factors

#### Common Flow Scenarios

- **Lead Scoring**: Classify leads by quality
- **Product Recommendation**: Suggest relevant products
- **Resource Allocation**: Prioritize resource assignment
- **Opportunity Classification**: Categorize sales opportunities

![Prediction and Classification](images/prediction-classification.png)

## Building and Training Custom AI Models

Create AI models tailored to your specific business needs without coding.

### Custom Model Creation Process

1. **Select Model Type**: Choose the type of model to build
2. **Collect Training Data**: Gather and organize your data
3. **Label Data**: Identify what the model should recognize
4. **Train Model**: Have AI Builder learn from your data
5. **Test and Refine**: Evaluate performance and improve
6. **Publish Model**: Make it available for use in flows
7. **Monitor and Update**: Maintain the model over time

### Best Practices for Custom Models

1. **Quality Training Data**: Use representative, high-quality data
2. **Sufficient Data Volume**: Provide enough examples (typically 50+ per category)
3. **Data Diversity**: Include variety that represents real-world scenarios
4. **Balanced Categories**: Provide similar numbers of examples per category
5. **Regular Retraining**: Update models as new data becomes available
6. **Continuous Testing**: Regularly verify model performance
7. **Clear Labeling**: Ensure consistent, clear data labeling

```
// Process for creating a custom classification model
1. Define business need (e.g., categorize customer feedback)
2. Collect 500+ examples of feedback
3. Create category labels (e.g., Product, Service, Website, Billing)
4. Label each feedback example with appropriate category
5. Train the model in AI Builder
6. Test with new feedback examples
7. Refine categories and retrain if needed
8. Publish the model for use in Power Automate
```

### Using Custom Models in Flows

Once created, your custom models appear as actions in Power Automate:

1. **Add AI Builder Action**: Select your custom model from the AI Builder actions
2. **Configure Input**: Provide the data to analyze
3. **Process Results**: Use model outputs in your flow logic

![Custom Model Creation](images/custom-model-creation.png)

## AI Ethics and Governance

Implement AI responsibly in your automation processes.

### Ethical AI Implementation

1. **Fairness**: Ensure AI doesn't discriminate or show bias
2. **Transparency**: Understand how the AI makes decisions
3. **Privacy**: Protect personal data used in AI models
4. **Human Oversight**: Maintain human review of critical AI decisions
5. **Accountability**: Establish clear responsibility for AI outcomes
6. **Limited Scope**: Use AI for appropriate tasks only

### AI Governance in Power Automate

1. **Access Control**: Restrict who can create/use AI models
2. **Data Loss Prevention**: Apply DLP policies to AI data
3. **Model Documentation**: Document model purpose and limitations
4. **Monitoring**: Track AI model usage and performance
5. **Regular Reviews**: Periodically evaluate model effectiveness
6. **Compliance Checks**: Ensure AI usage meets regulatory requirements

### Best Practices for AI in Business Processes

1. **Start Small**: Begin with limited-scope pilot projects
2. **Combine with Human Review**: Use AI to assist rather than replace
3. **Clear Success Metrics**: Define how to measure AI effectiveness
4. **User Training**: Ensure users understand AI capabilities and limitations
5. **Feedback Loops**: Create mechanisms for reporting AI issues
6. **Gradual Implementation**: Progressively expand AI use as confidence grows

## Advanced AI Scenarios

Explore sophisticated applications of AI in Power Automate.

### Multi-model AI Chains

Combine multiple AI models in sequence:

```
// Example: Comprehensive document processing
Trigger: Email with attachment received
   |
   v
AI Builder: Extract text from document
   |
   v
AI Builder: Detect language and translate if needed
   |
   v
AI Builder: Extract key phrases
   |
   v
AI Builder: Analyze sentiment
   |
   v
AI Builder: Classify document type
   |
   v
Create document record with all AI insights
   |
   v
Route based on classification and sentiment
```

### AI-powered Decision Flows

Use AI to make or recommend decisions:

```
// Example: AI-assisted loan application processing
Trigger: Loan application submitted
   |
   v
Get applicant details and history
   |
   v
AI Builder: Predict loan risk score
   |
   v
Condition: Risk score < threshold?
   |
   |---Yes---> Route for automated approval
   |
   |---No----> AI Builder: Classify decline reasons
                Route for human review with AI recommendations
```

### Hybrid Human-AI Processes

Create workflows that combine AI efficiency with human judgment:

```
// Example: Resume screening and hiring
Trigger: Resume uploaded to job application
   |
   v
AI Builder: Extract text from resume
   |
   v
AI Builder: Extract key qualifications and experience
   |
   v
AI Builder: Match against job requirements
   |
   v
Create candidate profile with match score
   |
   v
Condition: Match score > threshold?
   |
   |---Yes---> Route to hiring manager with AI analysis
   |            Schedule initial screening
   |
   |---No----> Send courtesy rejection
```

![Advanced AI Scenario](images/advanced-ai-scenario.png)

## Future Trends in AI and Power Automate

Emerging capabilities and directions for AI in business automation.

### Expanding AI Capabilities

- **Conversational AI**: More sophisticated natural language processing
- **Video Analysis**: Extract insights from video content
- **Anomaly Detection**: Identify unusual patterns in data or processes
- **Reinforcement Learning**: Models that improve through process interaction
- **Document Understanding**: More comprehensive document analysis
- **Multimodal AI**: Process multiple types of inputs (text, image, audio)

### AI-driven Process Optimization

- **Process Mining**: Discover and analyze existing processes
- **Intelligent Recommendations**: Suggest process improvements
- **Adaptive Workflows**: Flows that self-optimize based on outcomes
- **Predictive Process Monitoring**: Forecast process issues before they occur
- **Intelligent Automation Selection**: AI suggests automation opportunities

### Democratization of AI

- **No-code AI Building**: Easier custom model creation
- **Pre-built Solution Accelerators**: Industry-specific AI templates
- **AI Explanation Capabilities**: Better understanding of model decisions
- **Simplified Model Management**: Easier governance and maintenance
- **Guided AI Implementation**: Wizard-driven AI integration

In the next section, we'll explore Power Automate Desktop for robotic process automation (RPA). 