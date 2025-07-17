# RAG (Retrieval Augmented Generation)

## The Problem RAG Solves

AI models are trained on public data up to a specific cutoff date. They cannot access information beyond what they were trained on — this includes your private company data, recent information beyond their training cutoff, or any specialized knowledge not in their training dataset.

The question is: how can we leverage the AI model's reasoning capabilities on data it wasn't trained on? For example, helping customers find products in your catalog, or reasoning about your company's internal documents.

## How RAG Works

What we do is turn these private documents into smaller chunks (more on that later), use an embedding AI model to turn these chunks into vectors (a series of numbers that capture sentiment and meaning of these chunks), we store these vector chunks in a vector database. Then when the user asks a question about these private documents (such as: "Recommend me products for hiking Mount Everest"), we turn the user question into vectors (again capturing the question's sentiment and meaning), compare that vector with the vectors stored in our vector database, and then finally feed the model both the user question and the returned chunked vector (from the vector comparison).

![image](https://github.com/user-attachments/assets/567589c9-4282-4a86-86d5-d9ea098e909f)


**So from first principles: RAG is literally building a function, where when user asks a question, we turn the question into vectors, compare it with vectors in our vector database, then once we find the relevant vectors, we add it to the question with the prompt. It is that simple.**

## Why Chunking is Necessary

The reason why data is chunked is because of context window. There is a limitation on the context window, and if the data exceeds that, which in most cases it will, then we divide it into chunks. Vector searches also work better on focused information. Note that when we say vector search, we typically mean semantic word search, rather than traditional keyword search.

## Search Types: Vector vs. Keyword vs. Hybrid

**Vector search** is great, but might miss exact keyword matches (might be too smart and return related but not precisely matching content).

**Keyword search** is great for exact keyword matching. Of course, it can't understand semantic relationships.

This is why **Hybrid search** is common — it combines both techniques to get better overall retrieval quality. Documents are reranked by combined scores, giving you the best of both worlds.

There are actually chunking strategies to determine the most appropriate size of the chunk. There is a tradeoff: if it is too small it will lack context, but if it is too large it will dilute relevant information.

Some vector databases, like Azure AI Search as an example, handle the chunking for us. We'll do it with code in this example.

## RAG Indexing: The Foundation of Quality

The quality of your RAG system depends on how you index your data.

### What Indexing Is

When you have a large collection of data, searching for it can be a challenge. The most straightforward way is to use linear search, but at scale, this becomes too slow.

Indexing creates a separate data structure that maps a searchable criteria to the location of the data. So instead of scanning all data, we scan the index first to identify where the relevant data is stored and retrieve only these pieces.

### Real-World Example: Pharmaceutical Company

For example, a pharmaceutical company creates an index mapping:

```
"drug_interactions" → [doc_3, doc_127, doc_2891, doc_5643, doc_8901]
"clinical_trials" → [doc_45, doc_298, doc_3372, doc_7123]  
"adverse_reactions" → [doc_12, doc_567, doc_4321, doc_9876]
"pharmacokinetics" → [doc_78, doc_1456, doc_6789]
```

Then when a scientist searches for drug interactions, it checks the index, and only retrieves the relevant documents as opposed to searching for all of them.

### Database Indexing Example

In relational databases as an example, you can create an index which maps column values (like salary, name, address) to row locations (actual values).

Let's say you have a table with 50,000 employee staff records:

```sql
CREATE TABLE employees (
    emp_id INT,
    last_name VARCHAR(100),
    department VARCHAR(50),
    hire_date DATE,
    salary DECIMAL(10,2)
);
```

You can create an index on the last name:

```sql
CREATE INDEX idx_last_name ON employees(last_name);
```

This then creates a separate data structure that looks like this:

**Index Structure (idx_last_name):**
```
"Adams" → Row 15,847
"Brown" → Row 2,156
"Chen" → Row 41,923
"Davis" → Row 8,901
"Evans" → Row 33,412
... (sorted alphabetically)
"Wilson" → Row 7,234
"Young" → Row 28,567
```

Now, when you search for Adams last name as an example, you can quickly retrieve it.

### RAG Indexing Context

In the context of RAG, like in a relational database where we use indexes to avoid scanning every row, RAG systems use vector database indexes to avoid comparing your query against every single document chunk.

In RAG systems, indexing quality affects whether your AI finds the right information to answer questions. Poor indexing means the AI gets irrelevant or incomplete context which leads to wrong or useless responses.

## RAG Indexing: 3 Critical Layers

### 1. Document Chunking

How do you actually organize the data. You can do it by character limit, but that means you will most likely miss some context or critical information. Hence why semantic chunking is used as illustrated before. These semantic splitters use language models.

### 2. Vector Embedding (Converting Text to Numbers)

Generic embedding can retrieve bad results (for instance, "IP" in legal might not be understood as intellectual property). Domain-specific embeddings lead to better results, so like fine-tuned embedding model on company data.

### 3. Vector Database Indexing

HNSW is the industry standard.

## Relevance and Quality Control

One thing to note is that not all search results are actually relevant. You need to determine what constitutes relevant content.

There is a **relevance threshold** that you put for vector searches. If you put it too high (0.0) then it might miss some content but it is valuable in high precision scenarios where wrong info could be costly. If it is too low, it might include irrelevant context that confuses the model. You need a way to measure this as well.

**User satisfaction:** Are users getting useful information? How often are retrieved chunks actually relevant?

## Production Considerations

### Data Pipeline Challenges
- How often does your knowledge base change?
- Ensuring clean and accurate data enters the system
- Version management (handling document updates and deletions)

### Governance
- How long to keep search logs & user queries
- Track who searched what and when
