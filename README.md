Overview:

This project delivers a robust, stateful chatbot built on the LangChain framework and powered by OpenAI's GPT-4 Turbo model. The solution is an end-to-end implementation focusing on persistence, allowing for seamless, multi-turn conversations where the bot remembers context across sessions by leveraging an SQLite database for message history.

Key Features:

Stateful Conversation:  It Can Remembers previous turns using persistent chat history for context-aware responses.

LLM Integration: Utilizes the powerful GPT-4 Turbo model for high-quality, relevant, and creative responses.

Modular Architecture (LangChain): Built with LangChain primitives for a clear, flexible, and scalable design.

Database-Backed Memory: Implements a persistent chat memory using SQLite to store conversations across user sessions.

Custom Prompt Engineering: Uses a defined SystemMessage to set the chatbot's personality and role in the conversation.

Chatbot Pipeline & Architecture:

The chatbot follows a sophisticated LangChain Expression Language (LCEL) pipeline, encapsulated within a RunnableWithMessageHistory component to manage state across user sessions.

1.Model and Adapter Initialization
The pipeline begins by configuring the LLM.

A ChatOpenAI instance is created, explicitly set to use the gpt-4-turbo model and configured with a temperature=1 for more creative and varied responses.

2. Prompt Engineering and Context
A robust ChatPromptTemplate is defined to guide the conversation and facilitate memory injection.

System Message: A SystemMessage is included at the start of the prompt to define the chatbot's identity: "You are a chatbot having conversation with human".

History Placeholder: A MessagesPlaceholder with the variable name chat_history is used to dynamically insert the conversation history retrieved from memory into the prompt context for every new turn.

Human Input: A HumanMessagePromptTemplate captures the current user query ({human_input}).

3. Persistent Memory Management
Conversation history is persisted to an SQLite database, ensuring that if the user returns later, the context of their previous session is maintained.

Database Integration: The SQLChatMessageHistory class is used, configured with an SQLite connection, to handle saving and fetching messages.

Session-Based Retrieval: A helper function (get_session_message_history_from_db) is implemented to retrieve the correct history based on a unique session_id (mapped to a user_id).

4. Conversational Chain
The core conversation logic is orchestrated by the RunnableWithMessageHistory object.

This object intelligently intercepts the user's new input (human_input).

It retrieves the prior conversation using the configured session_id.

It combines the history, the prompt template, and the current input into the final payload sent to the gpt-4-turbo model.

After generating a response, it automatically saves the new exchange (user query + bot response) back to the SQLite history database.
