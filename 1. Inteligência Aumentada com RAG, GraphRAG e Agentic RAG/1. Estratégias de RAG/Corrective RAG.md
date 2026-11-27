Geração Aumentada por Recuperação Corretiva (Corrective RAG / CRAG) integra ao [[LLMs]] mecanismos de recuperação de informações com processos de autoavaliação e correção. O CRAG implementa etapas adicionais para avaliar e corrigir a qualidade das informações antes de utilizá-las na geração de respostas.

**Principais Características do CRAG**:
- Ações Corretivas Baseadas na Confiança: Dependendo do grau de confiança nos documentos recuperados, diferentes ações são desencadeadas:
	- Alta Confiança: 
		- Se pelo menos um documento excede o limiar de relevância, o sistema procede à geração de resposta utilizando as informações disponíveis.
	- Baixa Confiança: 
		- Se todos os documentos estão abaixo do limiar de relevância, o sistema realiza uma busca adicional na web para obter informações mais precisas e atualizadas.
	- Confiança Ambígua: 
		- Quando os documentos apresentam relevância mista, o sistema combina o refinamento das informações recuperadas com buscas adicionais para assegurar a precisão de resposta.
- Refinamento do Conhecimento: Um algoritmo de "decomposição e recomposição" é utilizado para segmentar os documentos recuperados em "fragmentos de conhecimento", permitindo que o modelo se concentre nas informações mais relevantes e filtre conteúdo irrelevantes ou redundantes.

**Vantagens do CRAG:**
- Robustez Aprimorada: 
	- Ao avaliar criticamente e corrigir as informações recuperadas, o CRAG reduz a probabilidade de gerar respostas baseadas em dados incorretos ou irrelevantes.
- Flexibilidade na Recuperação de Informações:
	- A capacidade de realizar buscas adicionais na web permite que o sistema acesse uma gama mais ampla de informações, garantindo respostas mais precisas e atualizadas;
- Integração Transparente: 
	- O CRAG é projeto para ser facilmente acoplado a diversas abordagens baseadas em [[RAG]], funcionando como um complemento que melhora a robustez e a precisão das respostas geradas.