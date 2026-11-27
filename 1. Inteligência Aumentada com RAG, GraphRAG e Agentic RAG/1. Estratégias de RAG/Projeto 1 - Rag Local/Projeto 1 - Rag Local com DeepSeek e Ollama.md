**Instalando o Ollama no WSL2:**
```bash
curl -fsSL https://ollama.com/install.sh | sh
```

**Usando LLM no formato padrão:**
- Vamos buscar o deepseek-r1 com menor parâmetro (1.5B).
- Vamos passar o comando `$ ollama run deepseek-r1:1.5b`
	1. O `ollama` irá fazer o download do modelo;
	2. Irá abrir a barra de conversação;

**Criando o ambiente virtual:**
1. Criar o ambiente virtual com python;
2. Ativar o ambiente virtual;
3. Instalar o pip
4. Instalar o `requirements.txt` que vem do repo da DSA.
	`$ pip install -r requirements.txt`
5. Executar o comando 
	`$ streamlit run dsarag.py`

**Construindo a Aplicação Web:**

```python
import streamlit as st
from langchain_community.document_loaders import PDFPlumberLoader
from langchain_huggingface import HuggingFaceEmbeddings
from langchain_experimental.text_splitter import SemanticChunker
from langchain_community.vectorstores import FAISS
from langchain_ollama import OllamaLLM
from langchain.chains.llm import LLMChain
from langchain.prompts import PromptTemplate
from langchain.chains.combine_documents.stuff import StuffDocumentsChain
from langchain.chains import RetrievalQA
import warnings

warnings.filterwarnings('ignore')

# Configurando o titulo da pag)
st.set_page_config(page_title="LLM com RAG Local", page_icon="🤖", layout="centered")

# Titulo
st.title("Sistema de RAG Local (DeepSeek e Ollama)")
st.markdown("---")

# Sidebar
with st.sidebar:
    st.header("Instruções:")
    st.markdown("""
    1. Faça upload de um arquivo PDF.
    2. Aguarde o carregamento e faça perguntas.
    3. O sistema vai recuperar o conteúdo relevante e a IA vai fornecer uma resposta.
    """)

# Caixa de upload para pdf
uploaded_file = st.file_uploader("Upload de Documento PDF", type='pdf')
  
# Se tiver arquivo iniciamos o processamento:
if uploaded_file is not None:
    # Mensagem
    st.success("PDF carregado com sucesso! Aguarde o processamento e digite sua pergunta!")

    # Salvar o arquivo em disco
    with open("arquivo.pdf", "wb") as f:
        f.write(uploaded_file.getvalue())

    # Leitura do arquivo pdf
    loader = PDFPlumberLoader("arquivo.pdf")

    # Permite ler o conteudo do arquivo
    docs = loader.load()

    # Divide os dados de texto em chunks
    text_splitter = SemanticChunker(HuggingFaceEmbeddings())
    documents = text_splitter.split_documents(docs)

    # Interessante para ver o SemanticChuncker
    print(documents)

    # Definindo o modelo de embeddings
    embedder = HuggingFaceEmbeddings()
    
    """Analise de embeddings"""
    # Extraindo o texto de cada chunk
    texts = [doc.page_content for doc in documents]
    # Gerando os embeeddings
    embeddings = embedder.embed_documents(texts)
    # Visualizando os embeddings
    for i, embedding in enumerate(embeddings):
        print(f"Chunk {i}")
        print(f"Texto: {texts[i]}")
        print(f"Embedding: {embedding}")
        print(f"Dimensão: {len(embedding)}")
    """Fim analise de embedding"""

    # Cria o banco de dados vetorial em memoria
    vectorDB = FAISS.from_documents(documents, embedder)

    # Cria o retriever
    retriever = vectorDB.as_retriever(search_type="similarity", search_kwargs = {"k":2})

    # Instância do LLM
    llm = OllamaLLM(model="deepseek-r1:1.5b")

    # Define o prompt
    prompt = """
    1. Responda apenas em Português do Brasil \n
    2. Se não souber a resposta, apenas diga "Eu não sei", não invente resposta. \n
    3. Mantenha a resposta concisa e limitada a 3 ou 4 parágrafos.
    Contexto: {context}
    Pergunta: {question}
    Resposta:
    """

    # Cria o prompt template
    prompt_template_geral = PromptTemplate.from_template(prompt)

    # Cria a cadeia de operações
    llm_chain = LLMChain(llm=llm, prompt=prompt_template_geral, verbose=True)

    # Cria o prompt específico com o contexto
    mem_prompt = PromptTemplate(input_variables = ['page_content', 'source'],
                                template="Contexto:\n
	                                content:{page_content}\n
	                                source:{source}")

    documents_chain = StuffDocumentsChain(llm_chain =llm_chain,
                                          document_variable_name="context",
                                          document_prompt=mem_prompt,
                                          verbose=True)

    qa_retrieval = RetrievalQA(
					combine_documents_chain = documents_chain,
                    retriever = retriever,
                    verbose=True,
                    return_source_documents=True)

    # User input
    user_input = st.text_input("Digite sua pergunta relacionada ao documento aqui:")
  
    # Processa o input e gera a resposta
    if user_input:
        with st.spinner("A IA está processando sua pergunta..."):
            try:
                response = qa_retrieval(user_input)["result"]
                st.success("Resposta do DeepSeek:")
                st.write(response)
            except Exception as e:
                st.error(f"Ocorreu um erro: {e}")
    else:
	    st.info("Faça o upload de um documento PDF para começar.")
```

