# ChromaDB_reading_from_multiple_sources
This demo's Collating data from different sources  (.txt/text files), storing it into Chroma DB and later querying questions from both the files in the same python script.

###  If you are using PDF input files, then adopt the same code below, but use the PDF Uploader library as shown in this link -> https://github.com/tonykipkemboi/ollama_pdf_rag/blob/main/local_ollama_rag.ipynb

Back to this code:

I have here multiple files/documents.txt and is read into the Chroma DB and the first query is from first file and the second query is from the second file.

Both the documents are loaded into the DB in the same code.

First file is State of the Union.txt, where references to what the President says about the US Supreme court nominee Ketanji Brown, and the second input file is State of the Onion.txt, where references to yinyangs are made.

There are two queries in the same Python script, one query is from the first input file, and second from second input file.

```

# query first document
query = "What did the president say about Ketanji Brown Jackson"
docs = db.similarity_search(query)

# print results
print ("Printing the results about Ketanji(first document) :");
print(docs[0].page_content)
print ("");


# query second document
query = "What does the yinyangs represent ?"
docs = db.similarity_search(query)

# print results
print ("Printing the results about yinyangs : ");
print(docs[0].page_content)
print ("");
exit()


```

![image](https://github.com/kiranshashiny/ChromaDB_reading_from_multiple_sources/assets/14288989/bf0b93bf-adb2-4c74-9510-5fa71b1935e7)

1. Snapshot of the White House publication regarding confirmation of Judge Jackson's appointment to the US Supreme Court, wherein remarks by the President is queried.
   
![image](https://github.com/kiranshashiny/ChromaDB_reading_from_multiple_text_documents/assets/14288989/45db8be6-128c-405f-ab45-d487cf016c56)


3. Snapshot of the text file containing YinYangs

![image](https://github.com/kiranshashiny/ChromaDB_reading_from_multiple_sources/assets/14288989/3d66f4af-5639-48ae-94c9-6826ad84c702)



```

$ cat langchain.py
# import
from langchain_chroma import Chroma
from langchain_community.document_loaders import TextLoader
from langchain_community.embeddings.sentence_transformer import (
    SentenceTransformerEmbeddings,
)
from langchain_text_splitters import CharacterTextSplitter

# load the document and split it into chunks
loader = TextLoader("state_of_the_union.txt")
documents = loader.load()

# split it into chunks
text_splitter = CharacterTextSplitter(chunk_size=1000, chunk_overlap=0)
docs = text_splitter.split_documents(documents)

# create the open-source embedding function
embedding_function = SentenceTransformerEmbeddings(model_name="all-MiniLM-L6-v2")

# load it into Chroma
db = Chroma.from_documents(docs, embedding_function)

## Added from here: shashi
# load the document and split it into chunks
loader = TextLoader("state_of_the_onion.txt")
documents = loader.load()

# split it into chunks
text_splitter = CharacterTextSplitter(chunk_size=1000, chunk_overlap=0)
docs = text_splitter.split_documents(documents)

# create the open-source embedding function
embedding_function = SentenceTransformerEmbeddings(model_name="all-MiniLM-L6-v2")

# load it into Chroma
db = Chroma.from_documents(docs, embedding_function)

# Added Upto here : Shashi

# query it
query = "What did the president say about Ketanji Brown Jackson"
docs = db.similarity_search(query)

# print results
print ("Printing the results about Ketanji : shashi");
print(docs[0].page_content)
print ("");

print ("done");
# query it
query = "What does the yinyangs represent ?"
docs = db.similarity_search(query)

# print results
print ("Printing the results about yinyangs : shashi");
print(docs[0].page_content)
print ("");
exit()

```

References:
https://realpython.com/chromadb-vector-database/#get-started-with-chromadb-an-open-source-vector-database

![image](https://github.com/kiranshashiny/ChromaDB_reading_from_multiple_text_documents/assets/14288989/e5319eb2-78d4-436a-9581-0a3ac5951765)


https://www.datacamp.com/tutorial/chromadb-tutorial-step-by-step-guide

https://docs.trychroma.com/guides

https://python.langchain.com/v0.2/docs/integrations/platforms/
