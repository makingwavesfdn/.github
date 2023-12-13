## Hi there 👋

#### Chatbot v3.3

An inbound SMS is received by Twilio

Twilio passes the event via webhook to AWS Lambda 9458617b-a4da-46de-83b0-cf276f13a2ac
- Source code for AWS Lambda 945 is deployed from GitHub Repository V-3.3-001-C-945

AWS Lambda 9458617b-a4da-46de-83b0-cf276f13a2ac
- Decodes the inbound SMS from base64 to utf-8
- Queues the SMS in AWS Simple Queue Service 9664e1ef-16bd-482a-a306-3065771ca665.fifo
- Returns a required callback, where callback is blank, to Twilio

AWS Simple Queue Service 966 triggers AWS Lambda c57bf700-81ed-41dd-95d9-04116488694f
- Source code for AWS Lambda c57 is deployed from GitHub Repository V-3.3-002-B-c57

AWS Lambda c57bf700-81ed-41dd-95d9-04116488694f
- Routes the inbound SMS based on a decision from a keyword orchestrator
  - If decision === 0, then the inbound SMS is sent to module Process Stack Response
    - Gets conversation history from AWS DynamoDB Table 4d16cb38-d21c-40ff-b33e-29bf273db051
    - Passes inbound SMS and conversation history to Stack-AI --> Production --> FAFSA Knowledge Base
    - Performs a retrieval augmented generation on the FAFSA Knowledge Base
    - FAFSA Knowledge Base last updated on December 12, 2023
    - Returns an SMS response via Twilio
    - Counts the number of messages in the user's conversation history
    - If the number of messages exceeds the max, then deletes the two oldest messages
      - Max number of messages is configured as an environmental variable on AWS Lambda c57
    - Adds the inbound SMS and response to the user's conversation history
  
  - If decision === 1, then the inbound SMS is sent to module Process Dall-E Response
    - Passes inbound SMS to OpenAI Dall-E 2 // Does not get conversation history
    - Performs an image generation
    - Receives an OpenAI image URL from Dall-E 2
    - Downloads the generated image via axios.get
    - Creates a unique name for the generated image via UUID
    - Uploads the generated image to AWS S3 --> Buckets --> making-waves --> images
    - Creates an AWS S3 public image URL
    - Returns an MMS response via Twilio, where mediaUrl = AWS S3 public image URL
    - Counts the number of messages in the user's conversation history
    - If the number of messages exceeds the max, then deletes the two oldest messages
      - Max number of messages is configured as an environmental variable on AWS Lambda c57
    - Creates a text description of the generated image, based on the original inbound SMS
    - Adds the original inbound SMS and text description of the generated image to the user's conversation history

  - If decision === 2, then the inbound SMS is sent to module Process GPT Response
    - Gets conversation history from AWS DynamoDB Table 4d16cb38-d21c-40ff-b33e-29bf273db051
    - Passes inbound SMS and conversation history to OpenAI GPT-4-1106-Preview
    - Performs a text completion
    - Returns an SMS response via Twilio
    - Counts the number of messages in the user's conversation history
    - If the number of messages exceeds the max, then deletes the two oldest messages
      - Max number of messages is configured as an environmental variable on AWS Lambda c57
    - Adds the inbound SMS and response to the user's conversation history

- Deletes the inbound SMS from AWS Simple Queue Service 9664e1ef-16bd-482a-a306-3065771ca665.fifo
- Writes a conversation summary to AWS DynamoDB Table 4d16cb38-d21c-40ff-b33e-29bf273db051
    - Conversation summary is not retrieved by v3.3
    - Conversation summary has a planned use for a variable reward engine in v3.4
    - v3.4 is kept, but not deployed, in GitHub Repository V-3.4-001-A-c57

<!--

**Here are some ideas to get you started:**

🙋‍♀️ A short introduction - what is your organization all about?
🌈 Contribution guidelines - how can the community get involved?
👩‍💻 Useful resources - where can the community find your docs? Is there anything else the community should know?
🍿 Fun facts - what does your team eat for breakfast?
🧙 Remember, you can do mighty things with the power of [Markdown](https://docs.github.com/github/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax)
-->
