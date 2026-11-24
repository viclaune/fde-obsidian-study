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
# Estudo de Caso - Estratégias de RAG e Suas Variantes - Sistema de RAG Local com DeepSeek e Ollama 

# Imports 
import streamlit as st # Streamlit é um framework web para linguagem python 
from langchain_huggingface import HuggingFaceEmbeddings 
from langchain_community.document_loaders import PDFPlumberLoader 
from langchain_experimental.text_splitter import SemanticChunker 
from langchain_community.vectorstores import FAISS 
from langchain_ollama import OllamaLLM 
from langchain.chains.llm import LLMChain 
from langchain.prompts import PromptTemplate 
from langchain.chains.combine_documents.stuff import StuffDocumentsChain from langchain.chains import RetrievalQA 
import warnings 
warnings.filterwarnings('ignore') ​

# Configurando o título da página e outras configurações (favicon)
st.set_page_config(page_title = "Data Science Academy", page_icon = ":100:", layout = "centered") 

# Título 
st.title("Estudo de Caso - Estratégias de RAG e Suas Variantes - Sistema de RAG Local com DeepSeek e Ollama") 
st.markdown("---")

# Sidebar com as instruções 
with st.sidebar: 
	st.header("Instruções") 
	st.markdown(""" 
	1. Faça upload de um arquivo PDF. 
	2. Aguarde o carregamento e faça perguntas. 
	3. O sistema vai recuperar o conteúdo relevante e a IA vai fornecer uma resposta. """)

# Cria a caixa de upload para o arquivo pdf 
uploaded_file = st.file_uploader("Upload de Documento PDF", type = "pdf") ​
```
