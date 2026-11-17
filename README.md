# Agente de IA para atendimento de imobiliária por e-mail (n8n)

> 📚 **Projeto de estudo** desenvolvido acompanhando o curso **"Agentes de IA e n8n"** da **Hashtag Treinamentos**. A empresa "Nova Lar Imóveis" é fictícia e foi criada para o exercício.

Agente de IA no **n8n** que lê os e-mails de clientes interessados em imóveis, consulta a planilha de imóveis disponíveis, responde de forma personalizada e registra ou atualiza o cliente em uma planilha de leads (CRM).

## Como funciona

```
Gmail Trigger → AI Agent → Reply to a message
                  ├── Modelo: Google Gemini (principal) + OpenAI (fallback)
                  ├── Memória: Simple Memory (por conversa)
                  └── Ferramentas: buscar_imoveis · registrar_lead (Google Sheets)
```

1. **Gmail Trigger**: verifica a caixa de entrada a cada hora e pega até 6 e-mails novos por vez.
2. **AI Agent ("Ana")**: recebe o texto do e-mail do cliente (`{{ $json.text }}`) e segue a **System Message**, que define quem ela é, o tom de voz, as informações da empresa, as regras e quando usar cada ferramenta.
3. **Ferramentas (Google Sheets)**:
   - `buscar_imoveis`: consulta a planilha de imóveis antes de recomendar qualquer opção, para a IA não inventar imóveis.
   - `registrar_lead`: usa **Append or Update** para criar o lead ou atualizar a linha de um cliente que já existe, sem duplicar. Os campos (nome, telefone, interesse, região, orçamento…) são preenchidos pela IA com `$fromAI()`.
4. **Simple Memory**: a chave é o `threadId` do Gmail, então cada conversa tem a sua própria memória (últimas 5 interações) e a IA não mistura um cliente com outro.
5. **Fallback de modelo**: se o Gemini falhar (indisponível ou limite de uso), o OpenAI assume e o fluxo não para.
6. **Reply to a message**: responde no mesmo e-mail, em **HTML**, para manter a formatação.

## O que aprendi
- Separar **o que não muda** (System Message: papel, regras, dados da empresa) do **que muda** (o texto de cada e-mail).
- Dar **ferramentas** à IA em vez de colocar todos os dados no prompt, para ela consultar informações atualizadas.
- Usar memória por conversa e um modelo de reserva (fallback).

## Como usar
1. No n8n, use **Import from File** para importar o arquivo `.json` deste repositório.
2. Conecte as credenciais do **Gmail**, do **Google Sheets**, do **Google Gemini** e da **OpenAI**.
3. Nos nós `buscar_imoveis` e `registrar_lead`, aponte para as suas planilhas de imóveis e de leads.

## Tecnologias
n8n · AI Agent · Google Gemini · OpenAI · Google Sheets · Gmail · engenharia de prompt

---
Autor: Vitor Alexandre Laboissiere Pereira · [LinkedIn](https://www.linkedin.com/in/vitorlaboissiere)
