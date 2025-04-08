### 🧠 What is `pgvector`?

**pgvector** is a PostgreSQL extension that adds support for **vector data types**—commonly used for storing **embeddings** from models like OpenAI, BERT, or CLIP. It enables efficient **similarity search** (e.g., cosine, L2, inner product) directly in your Postgres database.

It’s ideal for building **semantic search**, **recommendation**, or **RAG (Retrieval-Augmented Generation)** systems — without needing a separate vector DB like Pinecone or FAISS.

---

### 🔧 How to start
```shell
docker compose -f pgvector/pgvector-compose.yml up
```

### 🚀 How to Use pgvector

#### 1. **Install the Extension**
```sql
CREATE EXTENSION IF NOT EXISTS vector;
```

#### 2. **Create a Table with a Vector Column**
```sql
CREATE TABLE items (
  id serial PRIMARY KEY,
  content text,
  embedding vector(1536)  -- assuming OpenAI's embedding size
);
```

#### 3. **Insert Data**
```sql
INSERT INTO items (content, embedding)
VALUES ('hello world', '[0.12, 0.32, ...]');  -- use actual embedding values
```

#### 4. **Query: Find Nearest Neighbors**
```sql
SELECT id, content
FROM items
ORDER BY embedding <-> '[0.11, 0.30, ...]'  -- query vector
LIMIT 5;
```
- `<->` is the cosine distance operator by default

---

### 💡 Tips
- Index support is available using **IVFFlat**, good for large-scale approximate search:
```sql
CREATE INDEX ON items USING ivfflat (embedding vector_cosine_ops) WITH (lists = 100);
```
- Choose `vector_cosine_ops`, `vector_l2_ops`, or `vector_ip_ops` depending on your similarity metric.
