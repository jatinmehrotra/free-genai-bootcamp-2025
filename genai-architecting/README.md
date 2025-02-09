## Business Requirements

- Japanese Language School wants to extend the language and improve the learning experience for students between instructor-led classes for their existing learning portal and learning record store.

- Their objectives are as follows:

  - build a collection of learning apps using various different use-cases of AI
  - Maintain the learning experience the learning portal using AI developer tools
  - Extend the platform to support various different languages

  ## Functional Requirements 

- The company had a budget of investing between 20-50K $. They are flexible as to implenting this solution on their own AI computer or use cloud managed services like AWS Bedrock
- They are more concerned on the privacy of user data and performance as they have around 250+ students at the moment.


## Non Functional Requirements

- Performance: Fast response time (<1 sec) for chat and speech feedback.
- Scalability: Support up to 10,000 concurrent users.
- Security: Ensure user data protection and compliance with regulations.
- Usability: Simple UI/UX for beginners.
- Availability: 99.9% uptime via managed cloud services.


## Assumptions

- AI-powered features will extend (not replace) the current platform.
- The target audience includes beginners and intermediate learners.
- The system will use both open-source models (e.g., Llama, Mistral) and managed services (e.g., AWS Bedrock).
- Cloud infrastructure will be used for scalability and cost-efficiency.
- Students interact through web/mobile apps.
- AI apps should support multiple languages, not just Japanese.

## Data Strategy
- Data Collection: Student interactions, pronunciation errors, chat history.
- Data Quality: Clean datasets to reduce AI biases.
- Integration: Sync with Learning Record Store (LRS) for personalized learning paths.




