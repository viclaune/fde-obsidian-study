```python
# Permite ler o conteúdo do arquivo
docs = loader.load() 
# Divide os dados de texto em chunks 
text_splitter = SemanticChunker(HuggingFaceEmbeddings()) 
documents = text_splitter.split_documents(docs)
```

Estamos dividindo o documento em chunks, ==🟢é preciso explorar as diferenças de chunks pois impacta no sucesso da [[RAG]]==. Nesse caso, estamos utilizando o `SemanticChunker()`, fazendo a divisão dos blocos de texto por uma ordem semântica (cada bloco de texto terá um comprimento fixo).

Para aplicar o Semantic Chuncker precisamos de um modelo de [[Embeddings]]. Com o `text_splitter` criado vamos aplicar em `docs` e gerar vários blocos de textos, estes vamos converter para representação numérica e guardar em [[Banco de Dados Vetorial]].