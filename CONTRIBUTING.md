# ✨ Contribuindo com este Projeto

## 🚀 Como Começar

### 1. Fork do Repositório

[Clique aqui para fork](https://github.com/CarlosBez-analista/crm-social-media-automation-platform/fork)

### 2. Clone Localmente

```bash
git clone https://github.com/SEU_USUARIO/crm-social-media-automation-platform.git
cd crm-social-media-automation-platform
```

### 3. Instalação de Dependências

```bash
npm install
# ou
pip install -r requirements.txt
```

### 4. Configuração do Ambiente

Crie um arquivo `.env` no diretório raiz:

```bash
cp .env.example .env
```

Preencha com suas credenciais:
- `WHATSAPP_API_TOKEN=your_token_here`
- `INSTAGRAM_GRAPH_APP_ID=your_app_id`
- `DATABASE_URL=postgresql://...`
- etc.

### 5. Build e Execução

```bash
npm run dev
# ou para Python
python src/main.py
```

---

## 📝 Diretrizes de Código

### Convenção de Nomeação

- **Arquivos:** Snake_case ou kebab-case (conforme linguagem)
- **Funções/Classes:** camelCase (JS/TS) ou snake_case (Python)
- **Variáveis:** snake_case (Python) ou camelCase (JS/TS)

### Estrutura de Pastas Sugerida

```
src/
├── crm/                 # Lógica principal do CRM
│   ├── connectors/      # Conectores de APIs externas
│   ├── agents/          # Agentes de IA
│   └── orchestrator/    # Coordenação entre serviços
├── services/            # Serviços de negócio
├── utils/               # Utilitários e helpers
└── config/              # Configurações

docs/                    # Documentação do projeto
tests/                   # Testes unitários/integração
examples/                # Exemplos de uso
scripts/                 # Scripts de automação
```

---

## 🧪 Rodando Testes

### Antes de fazer commit:

```bash
npm test
# ou para Python
pytest tests/
```

### Cobertura de Código

Para verificar a cobertura dos testes:

```bash
npm run coverage
```

---

## 💡 Exemplos de Contribuições

### 1. Novo Conector de API

Exemplo: Adicionar suporte ao **EvolutionGO**

```typescript
// src/crm/connectors/whatsapp/evolution-go-integration.ts
export class EvolutionGOConnector implements WhatsAppConnector {
  constructor(
    private readonly config: ConnectorConfig,
    private readonly token: string
  ) {}

  async sendMessage(message: Message): Promise<SendResult> {
    const response = await fetch('https://evolutionapi.com/api/v1/send', {
      method: 'POST',
      headers: {
        'Authorization': `Bearer ${this.token}`,
        'Content-Type': 'application/json'
      },
      body: JSON.stringify({
        phone_number_id: this.config.phoneNumberId,
        messaging_product: 'whatsapp',
        to: message.to,
        type: 'text',
        content: message.content
      })
    });

    return await response.json();
  }
}
```

### 2. Novo Agente AI de RAG

```typescript
// src/crm/agents/rag/customer-support-agent.ts
export class CustomerSupportAgent implements Agent {
  constructor(
    private readonly retriever: VectorRetriever,
    private readonly llm: LLMModel
  ) {}

  async handleQuery(query: string): Promise<Answer> {
    // 1. Buscar contexto relevante do RAG
    const context = await this.retriever.search(query, { topK: 3 });
    
    // 2. Gerar resposta com RAG
    const response = await this.llm.generate(
      `${query}. Use o seguinte contexto para responder: ${context}`
    );

    return { answer: response, sources: context };
  }
}
```

### 3. Sistema de Aprovação de Agentes

```typescript
// src/crm/agents/team-management/approval-system.ts
export async function approveAgent(agentId: string): Promise<boolean> {
  const agent = await agentRepository.getById(agentId);
  
  // Executar suite de testes
  const testResults = await runAgentTestSuite(agent);
  
  // Verificar pass/fail rate
  const failureRate = (testResults.total - testResults.passed) / 
                      testResults.total * 100;

  if (failureRate < 5) {
    // Agentes aprovados com >95% de taxa de sucesso
    await agentRepository.updateStatus(agentId, 'APPROVED');
    return true;
  } else {
    // Rejeitar e solicitar revisão
    await agentRepository.updateStatus(agentId, 'REVISION_NEEDED');
    throw new ApprovalRequiredError();
  }
}
```

---

## 🤝 Processo de Pull Request

1. **Crie um branch** do seu fork: `git checkout -b feature/nome-da-feature`
2. **Faça o commit** seguindo a [Convensão de Commits](https://www.conventionalcommits.org/)
3. **Empurre para o fork**: `git push origin feature/nome-da-feature`
4. **Crie um PR** no repositório original
5. **Espere revisão** e feedback da equipe

### Convensão de Commit

```bash
git commit -m "feat: adiciona conector EvolutionGO"
# ou
git commit -m "fix: corrige erro ao receber webhook"
```

---

## 📚 Recursos Úteis

- [LangChain Documentation](https://python.langchain.com/v0.2/docs/introduction/)
- [WhatsApp Business API Docs](https://developers.facebook.com/docs/whatsapp/business-api/)
- [Meta Graph API](https://developers.facebook.com/docs/graph-api/)
- [Twitter Developer Portal](https://developer.twitter.com/en/)

---

## 🐛 Reportando Bugs ou Pedindo Recursos

Para novos issues, siga este modelo:

```markdown
# Bug Report / Feature Request

## Preços de Submissão

Crie um novo issue se for o primeiro relator deste problema.

### Informações do Problema

Por favor inclua a seguinte informação:

- **Componente Afetado:** (Ex: Conector WhatsApp, Agente AI, etc)
- **Versão Atual:** X.X.X
- **Ambiente:** Sandbox/Produtiva

### 📋 Detalhes do Problema

Descreva claramente o problema e como ele se comporta. Inclua a expectativa vs. realidade.

### 💻 Código de Reprodução

Se aplicável, inclua um trecho de código mínimo que reproduza o problema.

```typescript
// Seu código aqui...
```

### 🔍 Logs e Erros

Cole os logs ou mensagens de erro completas para ajudar no diagnóstico.

---

## Feature Request

### ✨ Sobre a Funcionalidade

Descreva claramente o que você quer que o sistema faça e por quê.

### 🎯 Benefícios Esperados

Por que esta funcionalidade seria útil? Quem se beneficia e como?
```

---

## ❓ Perguntas Frequentes

**Q: Como faço para adicionar suporte a uma nova rede social?**  
A: Crie um novo arquivo na pasta `/src/crm/connectors/` implementando a interface `SocialMediaConnector`. Siga o exemplo nos outros conectores.

**Q: Posso usar LLMs diferentes nos agentes AI?**  
A: Sim! O sistema suporta classificação automática de providers baseada no contexto da consulta e custo-benefício.

**Q: Onde encontrar exemplos de prompts para agentes AI?**  
A: Veja a pasta `examples/` com templates de system prompts aprovados.

---

## 🙏 Agradecimentos

Obrigado por contribuir! Sua contribuição ajuda a construir uma plataforma de automação de atendimento mais robusta e escalável.
