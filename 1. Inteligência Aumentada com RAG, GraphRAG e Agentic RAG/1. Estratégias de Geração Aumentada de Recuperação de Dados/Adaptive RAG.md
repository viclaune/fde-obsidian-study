Geração Aumentada por Recuperação Adaptativa, é uma abordagem que aprimora os modelos de [[LLMs]] ao ==🟢ajustar dinamicamente a estratégia de recuperação de informações com base na complexidade da consulta==. 

**Principais Características do Adaptive RAG:**
- Classificação da Complexidade da Consulta: Utilizar um classificador treinado para avaliar a complexidade da pergunta e determinar a estratégia de recuperação mais apropriada.
- Seleção Dinâmica de Estratégica: Dependendo da avaliação da complexidade, o sistema pode optar por:
	- Sem Recuperação: Para perguntas simples, o modelo responde diretamente com base em seu conhecimento interno.
	- Recuperação Única: Para perguntas de complexidade moderada, realiza uma única etapa de recuperação de informações externas para complementar a respostas.
	- Recuperação Iterativa: Para perguntas complexas que requerem múltiplas fontes de informação, executa várias etapas de recuperação e integração de dados.

**Vantagens do Adaptive RAG:**
- Eficiência aprimorada: Ao adaptar a estratégia de recuperação à complexidade da consulta, evita sobrecarga computacional desnecessária em perguntas simples e garante profundidade suficiente em perguntas complexas.
- Precisão elevada: A seleção da estratégia mais adequada para cada consulta melhora a precisão das respostas fornecidas pelo modelo.
- Flexibilidade: A abordagem dinâmica permite que o sistema lide eficazmente com uma ampla gama de perguntas, desde as mais simples até as mais complexas.