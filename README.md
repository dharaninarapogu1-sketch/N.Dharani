# Agentic RAG-Based College Assistant

##  Project Overview

The **Agentic RAG-Based College Assistant** is an AI-powered college information assistant designed to help students retrieve accurate information from official college documents.

The system combines **Agentic AI**, **Retrieval-Augmented Generation (RAG)**, **vector database technology**, and a **local Large Language Model (LLM)** to provide context-based answers with relevant document source references.

Instead of generating answers only from general AI knowledge, the system retrieves information from college documents stored in the system and uses the retrieved content to generate responses.

---

##  Objectives

The main objectives of this project are:

* To provide students with quick access to college information.
* To retrieve information from official college PDF documents.
* To generate context-based and reliable answers.
* To reduce hallucinations by restricting responses to retrieved documents.
* To provide source references for generated answers.
* To provide an Admin Panel for managing college documents.
* To use Agentic AI for selecting the appropriate information-retrieval process.
* To maintain chat history for student interactions.
* To support different types of college information such as syllabus, notices, timetables, faculty information, and regulations.

---

##  Key Features

### 1. AI Chatbot

The chatbot allows students to ask questions in natural language and receive answers based on the college documents available in the system.

Example:

> "What are the subjects in III Year II Semester?"

The system retrieves the relevant information and generates an answer.

### 2. RAG-Based Question Answering

The system uses Retrieval-Augmented Generation to:

1. Receive the student's question.
2. Retrieve relevant document chunks.
3. Pass the retrieved information to the local LLM.
4. Generate an answer based on the retrieved context.
5. Display the relevant source document.

### 3. Agentic AI

An agent/router determines which information source or tool should be used to answer a student's query.

The system can handle areas such as:

* Syllabus
* Department information
* Faculty information
* Student information
* Class timetable
* Examination timetable
* Notices
* Uploaded college documents
* Chat history

### 4. Admin Panel

The Admin Panel allows authorized college administrators/faculty to upload and manage official college PDF documents.

Uploaded documents are:

* Stored securely.
* Processed and converted into text.
* Split into smaller chunks.
* Converted into vector embeddings.
* Stored in the vector database.
* Made available for retrieval by the chatbot.

Students do not directly manage the official college document collection.

### 5. Source References

The chatbot provides source information along with the answer so that users can identify the document from which the information was retrieved.

### 6. Local LLM

The project uses a locally running Hugging Face model instead of depending on paid cloud-based APIs.

This reduces dependency on external API keys and cloud-based LLM services.

### 7. Chat History

The system stores conversation information so that previous interactions can be maintained and referenced where required.

---

##  System Architecture

The overall workflow of the system is:

```text
                ┌───────────────────────┐
                │       Student         │
                │   Enters Question     │
                └───────────┬───────────┘
                            │
                            ▼
                ┌───────────────────────┐
                │      Streamlit UI     │
                └───────────┬───────────┘
                            │
                            ▼
                ┌───────────────────────┐
                │     Agent Router      │
                └───────────┬───────────┘
                            │
              ┌─────────────┼─────────────┐
              │             │             │
              ▼             ▼             ▼
          PDF RAG       College Tools   Chat History
              │             │
              ▼             ▼
       ┌─────────────┐   ┌─────────────┐
       │  ChromaDB   │   │   SQLite    │
       └──────┬──────┘   └─────────────┘
              │
              ▼
       ┌─────────────┐
       │ Local LLM   │
       └──────┬──────┘
              │
              ▼
       ┌─────────────┐
       │    Answer   │
       │ + Source    │
       └─────────────┘
```

---

##  Technologies Used

| Technology            | Purpose                                      |
| --------------------- | -------------------------------------------- |
| Python                | Main programming language                    |
| Streamlit             | Web application interface                    |
| LangChain             | RAG and LLM integration                      |
| LangGraph             | Agentic workflow                             |
| ChromaDB              | Vector database                              |
| Sentence Transformers | Text embeddings                              |
| Hugging Face          | Local LLM and embedding models               |
| PyPDF                 | PDF document processing                      |
| SQLite                | Structured data and chat history             |
| PowerShell            | Project execution and environment management |
| VS Code               | Development environment                      |

---

##  Models Used

### Embedding Model

The project uses a Sentence Transformer-based embedding model to convert document chunks and user queries into numerical vectors.

These vectors allow the system to perform semantic similarity search.

### Local Language Model

The project uses:

```text
Qwen/Qwen2.5-0.5B-Instruct
```

The model is used to generate responses using the information retrieved from the college documents.

---

##  Project Structure

```text
Agentic_RAG_Based_College_Assistant/
│
├── app.py
│
├── agent_router.py
├── llm_service.py
├── rag_service.py
├── database_service.py
│
├── admin_ui.py
├── chatbot_ui.py
│
├── tools/
│   ├── college_info_tool.py
│   ├── class_timetable_tool.py
│   ├── student_info_tool.py
│   ├── faculty_info_tool.py
│   ├── department_info_tool.py
│   ├── notice_tool.py
│   ├── exam_timetable_tool.py
│   └── chat_history_tool.py
│
├── data/
│   ├── uploads/
│   ├── chroma_db/
│   └── college.db
│
├── requirements.txt
│
└── README.md
```

> The exact file structure may vary depending on the final project version.

---

##  RAG Workflow

The document retrieval process works in the following stages:

### Step 1: Document Upload

An administrator uploads an official college PDF through the Admin Panel.

### Step 2: PDF Processing

The system extracts text from the uploaded PDF using PyPDF.

### Step 3: Text Chunking

Large documents are divided into smaller chunks using a text splitter.

### Step 4: Embedding Generation

Each text chunk is converted into a numerical vector using the embedding model.

### Step 5: Vector Storage

The embeddings and associated metadata are stored in ChromaDB.

### Step 6: Question Processing

A student enters a question through the chatbot.

### Step 7: Similarity Search

The question is converted into an embedding and compared with stored document embeddings.

### Step 8: Context Retrieval

The most relevant document chunks are retrieved.

### Step 9: Response Generation

The retrieved context is provided to the local LLM.

### Step 10: Final Answer

The chatbot generates an answer using the retrieved information and displays the relevant source reference.

---

##  Data Handling

The system uses document-level metadata to identify uploaded documents.

Important metadata can include:

* Document ID
* File name
* Page number
* Document type
* Semester
* Department
* Upload information

This metadata helps the system retrieve information from the correct document.

---

##  Admin Workflow

The Admin Panel follows this workflow:

```text
Admin Login
     ↓
Upload College PDF
     ↓
Validate PDF
     ↓
Extract Text
     ↓
Split Text into Chunks
     ↓
Generate Embeddings
     ↓
Store in ChromaDB
     ↓
Store Document Information
     ↓
Document Available to Chatbot
```

---

##  Student Workflow

```text
Student Opens Chatbot
          ↓
    Enters Question
          ↓
     Agent Router
          ↓
    Relevant Tool/RAG
          ↓
   Retrieve Information
          ↓
      Local LLM
          ↓
 Answer + Source Reference
```

---

##  Installation

### 1. Clone or Download the Project

Download the project and open the project folder in VS Code.

### 2. Create a Virtual Environment

On Windows PowerShell:

```powershell
python -m venv .venv
```

### 3. Activate the Virtual Environment

```powershell
.\.venv\Scripts\Activate.ps1
```

### 4. Install Dependencies

```powershell
python -m pip install -r requirements.txt
```

### 5. Run the Application

```powershell
streamlit run app.py
```

The Streamlit application will open in the browser.

---

##  Configuration

The project is designed to work with a local LLM and local vector database.

If environment variables are required, create a `.env` file in the project directory.

Example:

```text
MODEL_NAME=Qwen/Qwen2.5-0.5B-Instruct
```

Do not upload private API keys, passwords, or confidential college information to public repositories.

---

##  Example Questions

Students can ask questions such as:

```text
What are the subjects in III Year II Semester?

What is the examination timetable?

Who is the faculty for the CSE department?

What are the college regulations?

What is the class timetable?

What are the subjects for IV Year I Semester?

What is mentioned in the uploaded syllabus?

What are the important notices?
```

The actual answers depend on the official documents available in the system.

---

##  Testing

The project was tested through multiple stages including:

* PDF ingestion
* Text extraction
* Text chunking
* Embedding generation
* ChromaDB storage
* Semantic retrieval
* Local LLM generation
* RAG-based question answering
* Source reference generation
* Agent routing
* Chat history
* End-to-end document processing

---

##  Hallucination Reduction

The system is designed to reduce unsupported answers by instructing the chatbot to use retrieved college-document context.

When the requested information is not available in the uploaded document, the system can respond with a message indicating that the information was not found instead of generating unsupported information.

---

##  Advantages

* Provides quick access to college information.
* Uses official college documents as the knowledge source.
* Supports natural-language questions.
* Provides source references.
* Uses RAG to improve contextual responses.
* Uses an agent-based architecture.
* Supports administrator document management.
* Can operate with a local LLM.
* Reduces dependence on external cloud APIs.
* Can be extended with additional college information tools.

---

##  Future Enhancements

Future versions can include:

* Multi-language support.
* Voice-based interaction.
* Improved authentication and role-based access.
* More advanced local LLMs.
* Better document version management.
* Automatic document expiry and replacement.
* Advanced analytics for administrators.
* Mobile-friendly interface.
* Improved evaluation and accuracy monitoring.
* Support for additional document formats.

---

##  Project Team

**Project Title:** Agentic RAG-Based College Assistant

**College:** SBIT Engineering College

**Branch:** Computer Science and Engineering (CSE)

**Team Members:**

1. Narapogu Dharani
2. Boina Sahaja
3. Gangaraboina Rishitha
4. Pasupuleti Akshitha

**Project Guide:** Dr. Rahul Nawkhare

---

##  Project Description

The **Agentic RAG-Based College Assistant** provides an intelligent platform for retrieving and analyzing college information. It combines Agentic AI, Retrieval-Augmented Generation, vector search, document processing, and a local language model to generate reliable, context-based answers from official college documents.

The system provides two major interfaces: a **Chatbot** for students and an **Admin Panel** for authorized document management. This architecture enables the college to maintain its own document-based knowledge source while allowing students to interact with the information through natural-language queries.

---

##  License

This project is developed for academic and educational purposes.
