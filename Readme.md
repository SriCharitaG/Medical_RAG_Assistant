# Medical Diagnosis Chatbot (using Google Gemini Pro)
This particular application creates a vector store from a high-quality document for medical symptoms and diagnosis and analyzes multiline text provided by the user to give an assessment. I have leveraged the Jina AI embedding model available on Hugging Face, which can support a large 8192 sequence length, and Google's GenAI Pro model as the LLM model for retrieval and augmentation (RAG). I am providing a detailed description below regarding all the components used. 

## Loading the sentences and embeddings from file:  
- I have added pretrained embeddings on a high-quality document that are saved on an index file and loaded at runtime. 
- As I have used FAISS indexing, which creates a vector store, the corresponding sentences are stored in a CSV file and loaded from it.
- Possible addition: A second page to add more documents to tokenize and embed sentences which are then updated in the above saved files.

## Searching for top embedding from the data that match the User's query :
- The user input query is converted to query embeddings using the embedding model which supports large sequence lengths, allowing for long multiline comments.
- I am using a FAISS index which can be used to quickly and efficiently return relevant embeddings for the query embedding, returning the top k closest embeddings with a default k value of 5.
- I then extract the corresponding sentences for those embeddings and join them to form input text for the LLM.
- Alternative: Instead of using embedding, the document can be split into chunks with some overlap using LangChains text splitters. These splits can then be encoded instead of sentences. But they have not provided good results so far and not further testing.

## Retrieval-Augmented Generation (RAG) :
- For this purpose, I used Google's GenAI Pro model.
- Initially, I created a simple template to let the LLM know what I want as an output. Giving a somewhat detailed explanation is the key for the LLM to provide valid answers in return. The prompt template variable takes the input and the template as input parameters.
- I then created an LLM chain that takes the LLM model and prompt template as input. Used SimpleSequentialChain to make sequential calls to the model.
- To reduce the possibility of model hallucination, the LLM model is fed inputs a second time, giving two responses. 

## Running the application on Streamlit
- Streamlit is an open-source Python framework used to create interactive apps.
- The application currently takes multiline text as user input.
- The application can be deployed locally using the "streamlit run app.py" command.
- Possible addition: Due to the large model size, the application cannot be deployed on the free community cloud provided by Streamlit, and we can use an enterprise cloud service like Azure Web Apps for the same.

## Instructions for running on your system
You can follow the below instructions for running the application on your system:
0. Extract the folder, install Visual Studio Code, and import the mindwatch folder. Open a new terminal in Visual Studio Code and type the below commands in the terminal.
1. conda create -n venv python==3.10 -y (one-time setup)
2. conda activate venv (Need to activate before you run the application every time locally)
3. pip install -r requirements.txt (one-time setup, update the file in case new python modules need to be installed)
4. streamlit run med_app.py (run the application)