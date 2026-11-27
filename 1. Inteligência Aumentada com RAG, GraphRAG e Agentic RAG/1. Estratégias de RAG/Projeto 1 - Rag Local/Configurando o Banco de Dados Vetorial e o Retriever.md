No [[Projeto 1 - Rag Local com DeepSeek e Ollama]] estamos utilizando o `FAISS`, um [[Banco de Dados Vetorial]] que trabalha muito bem em memória, com alta velocidade. Por morar em memória, encerrando a aplicação, encerra o RAG.

O [[LLMs]] não vai acessar o [[Banco de Dados Vetorial]], nós que faremos o acesso através do [[Retriever]] e recuperamos os dados do [[Banco de Dados Vetorial]] e entregamos para a [[LLMs]]

Em um [[Banco de Dados Vetorial]], o [[Retriever]] é responsável por localizar e recuperar informações relevantes armazenadas na forma de vetores de alta dimensão, conhecidos como [[Embeddings]].

```python
vectordb = FAISS.from_documents(documents, embedder)

retriever = vectordb.as_retriever(search_type = "similarity", search_kwargs = {"k": 2})
```

`as_retriever()` com busca por `similarity` (similaridade matemática) com `"k":2` (encontrar 2 vetores mais parecidos com o vetor pergunta).

Quanto maior o número de documentos, melhores o contexto que será entregue ao LLM, melhores respostas, porém, tempos maiores também.

