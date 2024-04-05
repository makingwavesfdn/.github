## See what's in our GitHub

## AI v3.4 - release in April 2024
- MyStateMachine-33rm
  - Inbound messages from users are handled by this state machine
  - Webhook Handler is Lambda Function 15358804-ad40-463e-a6b3-36232e30bcb3
 
- MyStateMachine-dbd8
  - Webhook Handler is Lambda Function 50247cd4-f0b9-49ec-8fab-558dd4cb08b2
  - Receives a contact's payload from ActiveCampaign via webhook
  - Extracts a custom field value from the payload which must be defined as https.../[field_name]
  - Extracts a contact's email address and converts it to senderPhoneNumber, the primary key for DynamoDB
  - Retrieves the contact's conversation history and conversation summary from DynamoDB
  - Passes the conversation history and conversation summary to OpenAI to personalize the field value
  - Posts the personalized field value to a custom field [%NEXT] on ActiveCampaign


- MyStateMachine-769znuqgd
  - Receives a contact's payload from ActiveCampaign via webhook
  - Extracts a contact's email address and converts it to senderPhoneNumber, the primary key for DynamoDB
  - Extracts a custom field value from the payload which must be defined as https.../[field_name]
  - Appends the field value to the contact's conversation history on DynamoDB as an assistant message
  - Webhook Handler is Lambda Function 4f5b07f7-0e95-45ca-8896-f83ce214cc25

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
