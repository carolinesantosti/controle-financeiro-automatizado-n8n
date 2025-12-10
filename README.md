# 💰 Controle Financeiro Automatizado (n8n + Telegram + IA)

Este projeto implementa um **assistente financeiro automatizado via Telegram**, capaz de registrar despesas pessoais a partir de mensagens em linguagem natural, utilizando **n8n**, **IA (AI Agent)** e **Google Sheets**.

> Projeto desenvolvido com foco em automação real, validação de dados, tratamento de erros e boas práticas de versionamento.

---

## 🚀 Visão Geral

O usuário envia uma mensagem pelo Telegram, por exemplo:

> `Gastei R$ 86,41 com enfeite de parede no Mercado Livre`

O sistema automaticamente:

* Interpreta a mensagem com IA
* Extrai descrição, valor, categoria e data
* Valida se os dados são suficientes
* Registra a despesa no Google Sheets
* Atualiza o total gasto
* Retorna uma confirmação formatada no Telegram

---

## 🧱 Arquitetura do Projeto

### Tecnologias Utilizadas

* **n8n (Self-hosted)** – Orquestração dos fluxos
* **Telegram Bot** – Interface de entrada e saída
* **AI Agent / LLM** – Interpretação semântica das mensagens
* **Google Sheets** – Persistência dos dados e resumo financeiro

---

## 🔗 Estrutura dos Workflows

### ✅ Workflow Principal – `Controle_Financeiro_V2`

Fluxo lógico:

1. **Telegram Trigger** – Recebe a mensagem do usuário
2. **Processar Mensagem (IA)** – Extrai os dados em JSON
3. **Parsear JSON da IA** – Converte resposta em estrutura válida
4. **IF – Validação de Dados**

   * ❌ Dados inválidos → Envia mensagem de erro
   * ✅ Dados válidos → Continua o fluxo
5. **Google Sheets – Registrar Despesa**
6. **Google Sheets – Ler Resumo**
7. **Merge** – Une dados da despesa com o total
8. **Formatar Resposta**
9. **Telegram – Enviar Confirmação**

---

### 🚨 Workflow de Erros – `Controle_Financeiro_Error_Handler`

Responsável por notificar automaticamente falhas em produção.

Fluxo:

1. **Error Trigger**
2. **Telegram – Enviar Mensagem de Erro**

A mensagem de erro inclui:

* Nome do workflow
* Nó que falhou
* Mensagem do erro
* Data e hora

---

## 🧠 Processamento com IA

A IA recebe um prompt estruturado para:

* Extrair **Descrição**, **Valor** e **Categoria**
* Definir a **Data automaticamente**
* Retornar **somente JSON válido**, sem texto adicional

### Formato esperado:

```json
{
  "Data": "dd/MM/yyyy",
  "Descrição": "texto",
  "Valor": "R$00,00",
  "Categoria": "Alimentação | Transporte | Compras Online | Outros"
}
```

---

## ✔️ Validação de Entrada

Após o parse do JSON:

* Um nó **IF** verifica se:

  * Descrição não é nula
  * Valor é maior que zero

### Caso inválido

O bot responde:

> ⚠️ Não entendi sua despesa.
> Para registrar, preciso:
> • Descrição
> • Valor gasto

Inclui exemplo para o usuário corrigir.

---

## 📊 Google Sheets

### Aba: `Despesas`

Campos registrados:

* Data
* Descrição
* Valor
* Categoria

### Aba: `Resumo`

* Campo **Total** soma automaticamente todas as despesas
* O valor é lido diretamente pelo workflow

---

## 📩 Mensagem de Confirmação (Telegram)

```text
✅ Despesa registrada com sucesso!

📅 Data: 10/12/2025
📝 Descrição: Padaria
💰 Valor: R$ 12,26
📂 Categoria: Alimentação

📊 Total gasto até agora: R$ 98,67

Deseja adicionar mais alguma coisa?
```

---

## 🧩 Versionamento

Estratégia adotada:

* **V1** – Primeira versão funcional
* **V2** – Estável (produção)
* **V3** – Versão de testes e melhorias

Nenhuma alteração é feita diretamente em versões produtivas.

---

## ✅ Checklist de Produção

* [x] Workflow ativo e testado
* [x] Validação de entradas inválidas
* [x] Tratamento centralizado de erros
* [x] Respostas claras ao usuário
* [x] Versionamento aplicado

---

## 🔮 Próximas Evoluções (V3)

* Classificação semântica aprimorada de categorias
* Interpretação de datas relativas ("ontem", "anteontem")
* Confirmação para valores elevados
* Relatórios mensais automáticos

---

## 📌 Conclusão

Este projeto demonstra uma automação completa e funcional, com foco em confiabilidade, experiência do usuário e boas práticas de arquitetura — pronta para uso diário e evolução contínua.
