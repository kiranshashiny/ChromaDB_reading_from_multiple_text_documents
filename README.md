# ChromaDB_reading_from_multiple_sources
Collate data from different sources and query it.


I have here multiple files/documents.txt and is read into the Chroma DB and the first query is from first file and the second query is from the second file.

Both the documents are loaded into the DB at the same time.

First file is State of the Union.txt, where references to what the President says about the US Supreme court nominee Ketanji Brown, and the
Second file is State of the Onion.txt, where references to yinyangs are made.

There are two queries, one query is from the first file, and second from second file.


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
