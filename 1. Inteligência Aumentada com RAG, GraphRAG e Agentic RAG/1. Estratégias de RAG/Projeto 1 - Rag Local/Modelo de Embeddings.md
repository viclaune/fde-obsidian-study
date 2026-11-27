São ferramentas de aprendizado de máquina que transformam dados complexos (palavras ou frases), em representações vetoriais de alta dimensão. Esses vetores capturam o significado semântico dos dados, permitindo que informações semelhantes estejam próximas no espaço vetorial.

Um exemplo: palavras com significados relacionados terão representações vetoriais próximas umas das outras.

Ao converter texto em vetores numéricos, os modelos de [[Embeddings]] facilitam a análise e processamento de grandes volumes de dados textuais, permitindo que algoritmos identifiquem padrões e relações semânticas de maneira eficiente.

No [[Projeto 1 - Rag Local com DeepSeek e Ollama]] utilizamos o `HuggingFaceEmbeddings`, e criamos o `embedder` para converter os textos em representações numéricas e guardar no banco vetorials.

```python
# Divide os dados de texto em chunks
text_splitter = SemanticChunker(HuggingFaceEmbeddings())
documents = text_splitter.split_documents(docs)

# Define o modelo de embeddings
embedder = HuggingFaceEmbeddings()

# Cria o banco de dados vetorial em memória
vectordb = FAISS.from_documents(documents, embedder)
```