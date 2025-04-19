### AIM:
To design and implement a LangChain Expression Language (LCEL) expression that utilizes at least two prompt parameters and three key components (prompt, model, and output parser), and to evaluate its functionality by analyzing relevant examples of its application in real-world scenarios.

### PROBLEM STATEMENT:
The project aims to design and implement a LangChain Expression Language (LCEL) expression that connects a prompt, a model, and an output parser to process user queries. It should accept at least two prompt parameters and demonstrate both simple and complex chains, including a retrieval system for context-based answers. The functionality will be tested using real-world examples to show how LCEL can simplify building AI-powered applications.

### DESIGN STEPS:

#### STEP 1: Define the Parameters
Identify the parameters (topic and length) to allow dynamic customization of prompts.
#### STEP 2: Design the Prompt Template
Create a structured prompt template with placeholders for parameters.
#### STEP 3: Select the Model
Use an LLM, such as OpenAI's GPT, to process the prompt.
### STEP 4: Implement the Output Parser
Design an output parser to format and structure the model's output.
### STEP 5: Integrate Components into an LCEL Expression
Combine the prompt template, model, and output parser into a LangChain pipeline.
### STEP 6: Evaluate with Examples
Test the LCEL expression using multiple input values for topic and length.
### PROGRAM:
NAME: YASHWINI M
REGNO: 212223230249

Simple Chain
```
import os
import openai
from dotenv import load_dotenv, find_dotenv
_ = load_dotenv(find_dotenv()) # read local .env file
openai.api_key = os.environ['OPENAI_API_KEY']
from langchain.prompts import ChatPromptTemplate
from langchain.chat_models import ChatOpenAI
from langchain.schema.output_parser import StrOutputParser
prompt = ChatPromptTemplate.from_template(
    "tell me about {topic}"
)
model = ChatOpenAI()
output_parser = StrOutputParser()
chain = prompt | model | output_parser
chain.invoke({"topic": "dog"})
```
![image](https://github.com/user-attachments/assets/4e1b72e5-db1f-457b-9c31-cfb81983a9dd)
Complex Chain
```
from langchain.embeddings import OpenAIEmbeddings
from langchain.vectorstores import DocArrayInMemorySearch   
vectorstore = DocArrayInMemorySearch.from_texts(
    ["GGenerative AI is a type of artificial intelligence that creates new content, such as text, images, or code, based on learned patterns.","(LCEL)LangChain Expression Language (LCEL) is a declarative syntax for composing and chaining LangChain components efficiently."],
    embedding=OpenAIEmbeddings()
)
retriever = vectorstore.as_retriever()
retriever.get_relevant_documents("what is generative ai?")
retriever.get_relevant_documents("what is the full form of LCEL")
```
![image](https://github.com/user-attachments/assets/d5f47ce7-f46f-4486-9794-19431e80ad5b)
```
template = """Answer the question based only on the following context:
{context}
Question: {question}
"""
prompt = ChatPromptTemplate.from_template(template)
from langchain.schema.runnable import RunnableMap
chain = RunnableMap({
    "context": lambda x: retriever.get_relevant_documents(x["question"]),
    "question": lambda x: x["question"]
}) | prompt | model | output_parser
chain.invoke({"question": "what is the full form of LCEL?"})
```
![image](https://github.com/user-attachments/assets/e52798ab-b3c0-450b-987b-b9ed25805844)
```
inputs = RunnableMap({
    "context": lambda x: retriever.get_relevant_documents(x["question"]),
    "question": lambda x: x["question"]
})
inputs.invoke({"question": "what is the full form of LCEL?"})
```
![image](https://github.com/user-attachments/assets/a74d6c8d-b3bd-4b53-8f7d-d135729bb31a)

## OUTPUT
Simple Chain 
![image](https://github.com/user-attachments/assets/3dbc3625-2dd7-4e4a-bf0f-f0e54b2cb2f8)
Complex Chain
![image](https://github.com/user-attachments/assets/399ab412-d77a-475d-98dd-0ff7d7e34f0c)

### RESULT:
Thus, the LangChain Expression Language (LCEL) expression that utilizes two prompt parameters and three key components (prompt, model, and output parser) was designed and implemented successfully. And also evaluated its functionality by analyzing relevant examples of its application in real-world scenarios.
