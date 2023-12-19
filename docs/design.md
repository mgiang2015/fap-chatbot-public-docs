
# Design

This document highlights the design of the application. 

## High-level Design

![Relationship diagram of components](../Diagrams/relationship%20diagram.jpg)

- **Frontend** is a React app that is open to users. It interacts only with the backend through REST API calls.
- **Backend** (coded in python) handles the requests and acts as an interface between frontend and database. It contains a REST API that performs operation based on requests from the frontend.
  - **LLM** refers to Large Language Model - AI models that are trained to understand and generate natural language
  - **Langchain** is a framework that provides abstractions to work with LLMs. In this project we use Langchain to manipulate prompts to get the best possible answer from the LLM
  - **Organization database** refers to organization's shared documents.
  - **Mongo database** is a vector database that stores embeddings of all documents to make it easier for the chatbot to query them
- **Database** refers to a mysql database that stores all information about users, conversations and documents needed.

## Detailed Design

This section describes our features.

### Authentication

Our authentication is self-implemented,
with our own APIs (login, register, change password, verification) to handle any authentication-related actions.
These APIs are communicated between our React frontend and Python backend,
which will interact with MySQL database to store user information.
Python libraries(brypt, uuid) are used to protect user passwords and send verification codes.
Verification codes are sent using smtp library using an Outlook email account.
Another consideration is to use Microsoft Outlook API to authenticate.

Here are the instructions for setting up the Outlook email account to send emails:
1. Follow the steps in this link to create an App Password for the email account you want to send emails with and remember the password shown.
https://support.microsoft.com/en-us/account-billing/manage-app-passwords-for-two-step-verification-d6dc8c6d-4bf7-4851-ad95-6d07799387e9
2. Set the `SENDER_MAIL, SENDER_MAIL_APP_PASSWORD` in .env file.

### API

We used FastAPI to create the API since it allows us to make async requests, is scalable, faster, and well supported and documented. The API allows the frontend to get information from the database and connects to other parts of the backend that generate the response for the user queries.

### Document Query

This is the vector database structure:

![Relationship diagram of components](../Diagrams/vectorstore%20architecture.jpg)

We can break this down to two main stages

#### Upload files: 
1. When a file is uploaded, it will be split into small chunks and the chunk's length will be less than 500 characters. Currently, the splitter we are using is RecursiveCharacterTextSplitter and the separators are ["\n\n", ".", ";", ":"] to keep the context as much as possible. In case the chunk's length is bigger than 500 char, its splitted further using ["\n\n", "\n", " ", ""] separators.
2. Each chunk will be cleaned to get a cleaner text for embedding
3. The chunks are passed through a text embedding model to get vector representations of the chunks
4. The vector representations obtained in the previous step will be stored in a vector database 
5. Using K means, we find the representatives of 20 clusters from the vector space. The representatives will be passed to OpenAI to generate summary
6. The summary is split into small chunks and steps 2, 3 are performed to get vector representations of the summary. The vector representations will be stored in vector database

#### Query files
1. The user's query will be paraphrased into 5 queries using OpenAI to get multiple perspectives from the original input and the queries will be cleaned to get better embeddings
2. The cleaned queries will be put into the first layer of filter, summary
3. After narrowing down the files, the queries are put into each file to get the most similar chunks. All the chunks are sorted using similarity score and top 5 chunks are taken
4. For each chunk, we will try to merge it with adjacent chunks to get a richer context
5. All the merged chunks are put into OpenAI to synthesize the answer

### Spelling/Grammar checking

Users can perform grammar and spelling checks for their uploaded files. The chatbot will respond with a table that contains the original and corrected text as well as their corresponding location(pdf-page number, ppt-slide number, doc-paragraph number) and filename. This feature works only for pdf, ppt and doc file extensions. At least 1 file must be selected.

