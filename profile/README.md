## See what's in our GitHub

## Copilot v3.4 - release between April 1 and June 30, 2024

- MyStateMachine-33rml899y - in production
  - Webhook Handler is Lambda Function 15358804-ad40-463e-a6b3-36232e30bcb3
  - Receives an SMS payload from Twilio via webhook
  - Extracts the senderPhoneNumber and body
  - Passes the body through moderation layers
    - Blocked Sender Filter
    - Self-Harm Filter with Administrator Notification via SMS
    - Validate or Create Conversation History and Conversation Summary on DynamoDB
    - PII Filter
  - Passes the body to conduct a web search with Brave
  - Retrieves the contact's conversation history and conversation summary from DynamoDB
  - Passes the web search results, conversation history, and conversation summary to OpenAI to generate a personalized response 
  - Passes the generated response through moderation layers
    - Keyword and Phrase Content Moderation Filter
    - LLM Content Moderation Filter
  - Sends the generated response to the senderPhoneNumber via Twilio
  - Appends the body of the original inbound message and generated response to conversation history on DynamoDB
  - Passes the body of the original inbound message and generated response to OpenAI to generate a new conversation summary
  - Replaces the old conversation summary on DynamoDB with the new conversation summary
 
- MyStateMachine-dbd8e6wyk - in production
  - Webhook Handler is Lambda Function 50247cd4-f0b9-49ec-8fab-558dd4cb08b2
  - Receives a contact's payload from ActiveCampaign via webhook
  - Extracts a custom field value from the payload which must be defined as https.../[field_name]
  - Extracts a contact's email address and converts it to senderPhoneNumber, the primary key for DynamoDB
  - Retrieves the contact's conversation history and conversation summary from DynamoDB
  - Passes the conversation history and conversation summary to OpenAI to personalize the field value
  - Posts the personalized field value to a custom field [%NEXT] on ActiveCampaign

- MyStateMachine-769znuqgd - in production
  - Webhook Handler is Lambda Function 4f5b07f7-0e95-45ca-8896-f83ce214cc25
  - Receives a contact's payload from ActiveCampaign via webhook
  - Extracts a custom field value from the payload which must be defined as https.../[field_name]
  - Extracts a contact's email address and converts it to senderPhoneNumber, the primary key for DynamoDB
  - Appends the field value to the contact's conversation history on DynamoDB as an assistant message
 
- MyStateMachine-m980coqm6 - for exactly-once processing - in production
  - Webhook Handler is Lambda Function 7f56f446-65a7-425a-b7f5-5ea7f3f968a8
  - Receives a contact's payload from ActiveCampaign via webhook
  - Extracts a custom field value from the payload which must be defined as https.../[field_name]
  - Extracts a contact's email address and converts it to senderPhoneNumber
  - Places senderPhoneNumber and field value in Simple Queue Service 7f56f446...
  - Simple Queue Service triggers Lambda Function a369e7c7-7923-4bc9-aaac-e95d884b4d31
  - Adds a receiptHandle, queueURL, and messageDeduplicationID then sends to State Function MyStateMachine-m980coqm6
  - Sends the message to the recipient via SMS from Twilio
  - Deletes the message from the Simple Queue Service 7f56f446...

- MyStateMachine-g2isew79h - in development
  - Webhook Handler is Lambda Function 2c6e4cf2-bf97-4d69-ad9a-d5714a365b62
  - Receives a contact's payload from ActiveCampaign via webhook
  - Extracts a contact's email address and converts it to senderPhoneNumber
  - Extracts a contact's contactID
  - Retrieves the contact's conversation history and conversation summary from DynamoDB
  - Passes the conversation history and conversation summary to Claude 2
  - Generates four next steps within an array
  - Posts each step within the array to a custom field on ActiveCampaign

## Chatbot v3.3 - release in January 2024
- GitHub Repository V-3.3-001-C-945
  - When a user texts the chatbot, the chatbot will route the prompt to the correct processor - Stack-AI, Dall-E 2, or GPT-4-1106-Turbo - and respond, via text, with context from previous conversations.
<br><br>
- GitHub Repository V-3.3-004-A-5d1
  - When an Active Campaign automation initiates a conversation with a user, the outbound message is added to the user's conversation history. When the user replies - even with an incomplete sentence - the chatbot will have context for providing a coherent response.
<br><br>
## Chatbot v3.4 - in development
- GitHub Repository V-3.4-001-A-c57
  - Building a variable reward engine for product-led growth.


<!--

**Here are some ideas to get you started:**

🙋‍♀️ A short introduction - what is your organization all about?
🌈 Contribution guidelines - how can the community get involved?
👩‍💻 Useful resources - where can the community find your docs? Is there anything else the community should know?
🍿 Fun facts - what does your team eat for breakfast?
🧙 Remember, you can do mighty things with the power of [Markdown](https://docs.github.com/github/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax)
-->
