📚 Projeto Empat-IA - Assistente de Saúde Mental Escolar
Equipe Empat-IA
Líder Técnico: William Serra
Scrum Master: Carlos Vinícius
Arquitetos: Daniel Machado, Verônica Rizzi
Desenvolvedores: Leandro Goes, Enei Pereira, Daniele Barbosa, Sonia Morbey, Vanderson Sa, Ely Santos

🎯 Objetivo do Projeto
A Empat-IA é uma solução inovadora que visa proteger a saúde mental de crianças em ambiente escolar, identificando padrões emocionais através de um chatbot interativo. Nosso foco principal são crianças que sofrem bullying e muitas vezes não conseguem expressar seus sentimentos aos adultos.

"Um trauma na infância pode marcar uma vida inteira. A Empat-IA está aqui para mudar essa história." ❤️

🛠️ Tecnologias Utilizadas
AWS Bedrock (Claude 3.5 Sonnet) - Para análise de sentimentos e interação conversacional

Python - Linguagem principal do projeto

Streamlit - Framework para construção da interface web

Boto3 - SDK da AWS para integração com serviços cloud

JSON - Para manipulação de dados

🌟 Funcionalidades Principais
Chatbot Empático 🤖💬

Conversa natural com alunos

Análise de sentimentos em tempo real

Respostas contextualizadas ao ambiente escolar

Monitoramento Contínuo 📊

Identificação de padrões emocionais

Alertas para educadores e pais

Sugestões de ações baseadas em IA

Interface Acessível 🎨

Design amigável para crianças

Responsivo e intuitivo

Personalização visual

📝 Código Fonte
python
import streamlit as st
import boto3
import json

# Configuração da sessão AWS
session = boto3.Session(profile_name="tcc-ia")
client = session.client('bedrock-runtime', region_name='us-west-2')

# Função para chamada ao AWS Bedrock
def call_bedrock_model(messages):
    payload = {
        "messages": [{"role": msg["role"], "content": msg["content"]} for msg in messages if msg["role"] in ["user", "assistant"]],
        "max_tokens": 500,
        "temperature": 0.7,
        "top_p": 0.9,
        "stop_sequences": ["--end"],
        "anthropic_version": "bedrock-2023-05-31"
    }

    try:
        response = client.invoke_model_with_response_stream(
            modelId="anthropic.claude-3-5-sonnet-20241022-v2:0",
            body=json.dumps(payload).encode("utf-8"),
            contentType="application/json",
            accept="application/json"
        )
        output = []
        stream = response.get("body")
        if stream:
            for event in stream:
                chunk = event.get("chunk")
                if chunk:
                    chunk_data = json.loads(chunk.get("bytes").decode())
                    if chunk_data.get("type") == "content_block_delta":
                        delta = chunk_data.get("delta", {})
                        if delta.get("type") == "text_delta":
                            output.append(delta.get("text", ""))
        return "".join(output)
    except Exception as e:
        return f"Erro ao chamar o modelo: {str(e)}"

[... código continua ...]
🏗️ Arquitetura da Solução
Diagram
Code






🚀 Como Executar o Projeto
Pré-requisitos:

Python 3.8+

Conta AWS configurada

Acesso ao AWS Bedrock

Instalação:

bash
pip install streamlit boto3
Execução:

bash
streamlit run empatia_chatbot.py
📊 Métricas de Avaliação
Métrica	Descrição	Meta
👩🎓 Taxa de Engajamento	Frequência de uso pelos alunos	≥70%
🎯 Precisão na Análise	Acerto na identificação de sentimentos	≥85%
⏱️ Tempo de Resposta	Velocidade média das interações	<2s
🛡️ Segurança de Dados	Conformidade com LGPD	100%
🤝 Como Contribuir
Faça um fork do projeto

Crie uma branch para sua feature (git checkout -b feature/AmazingFeature)

Commit suas mudanças (git commit -m 'Add some AmazingFeature')

Push para a branch (git push origin feature/AmazingFeature)

Abra um Pull Request

📜 Licença
Distribuído sob a licença MIT. Veja LICENSE para mais informações.

✉️ Contato
Projeto Link: https://github.com/willserra/Empat-IA

Made with ❤️ by Escola da Nuvem - Turma 12/2024
