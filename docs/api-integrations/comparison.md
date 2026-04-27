# 📚 Guia de Integração com APIs de Redes Sociais

## WhatsApp Business API

### Provedores Oficiais e Terceiros

#### 1. Meta WhatsApp Business API (Oficial)
- Documentação: https://developers.facebook.com/docs/whatsapp/business-api/
- Requisitos: Número verificado, sandbox ou ambiente produtivo
- Cota inicial de envio gratuita

#### 2. EvolutionGO
- URL Base: `https://evolutionapi.com/api`
- Endpoint principal: `/v1/send`
- Autenticação: Token no header

```javascript
// Exemplo básico de envio
await fetch('https://evolutionapi.com/api/v1/send', {
  method: 'POST',
  headers: {
    'Authorization': 'Bearer YOUR_TOKEN',
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({
    phone_number_id: 'PHONE_ID',
    messaging_product: 'whatsapp',
    to: '1234567890',
    type: 'text',
    content: 'Olá! Recebemos sua mensagem.'
  })
});
```

#### 3. Z-API
- Documentação disponível na página oficial
- Suporte a múltiplos ambientes
- Dashboard em tempo real

#### 4. UZAPI / NOTIFICAME HUB
- Soluções completas de automação
- Incluem painéis de controle
- Templates prontos para uso
- Integração direta com CRMs populares

---

## Instagram Graph API (Oficial)

### Autenticação via Meta for Developers
```javascript
const { graphAPI } = require('instagram-node');

// Login e obtenção de token
const response = await graphAPI.login(
  'APP_ID',
  'APP_SECRET',
  'ACCESS_TOKEN'
);
```

---

## Facebook Graph API (Oficial)

### Recursos Principais
- Mensagens via Messenger
- Posts em páginas
- Insights e analytics

```javascript
// Obter mensagens recentes
const response = await graphAPI.get(
  `/me/messages?limit=50`
);
```

---

## TikTok API (Limitada)

### Acesso Oficial
- Disponível apenas para marcas com ≥50k seguidores ou verificadas
- Sandbox para desenvolvimento

```javascript
const response = await tiktokAPI.get('/feed', {
  headers: { Authorization: `Bearer ${token}` }
});
```

---

## X (Twitter) API v2 (Oficial)

### Endpoint de Conversas Diretas
- Documentação: https://developer.twitter.com/en/docs/twitter-api
- Requer aprovação ou acesso via premium

```javascript
const tweets = await twitterAPI.conversations()
  .dmThreads() 
  .get({ queryParams: { limit: 10 } });
```

---

## Padrões de Integração Recomendados

### Arquitetura Unificada

```
┌─────────────────────────────────────────────┐
│         API Orchestrator Layer              │
│  ┌──────────┬──────────┬────────────────┐   │
│  │ WhatsApp │ Instagram│  Facebook     │   │
│  │          │          │                │   │
│  │ Provider A│Provider B│  Provider C  │   │
│  └──────────┴──────────┴────────────────┘   │
└─────────────────────────────────────────────┘
```

### Handler por Plataforma

```typescript
interface SocialMediaHandler {
  connect(provider: string, config: ConnectionConfig): Promise<void>;
  sendMessage(message: Message): Promise<Response>;
  receiveMessage(): AsyncGenerator<Message>;
}

abstract class PlatformHandler implements SocialMediaHandler {
  abstract connect(provider: string):
}
```

---

## Melhores Práticas de Implementação

1. **Use abstrações** para cada provedor - mantenha o código genérico
2. **Implemente fallbacks** quando uma API oficial falhar, use provedores alternativos
3. **Crie cache** de tokens para evitar reautenticação constante
4. **Monitore limites de uso** (rate limiting) de cada plataforma
5. **Padronize mensagens** entre plataformas
6. **Logs detalhados** de todas as interações

---

## Recursos Recomendados da Comunidade

### Repositórios Open-Source Relacionados

1. **public-apis/public-apis** - Lista de APIs públicas gratuitas
2. **Z-API/uzapi-web-interface** - Interface web para UZAPI
3. **evolution-api-whatsapp** - Biblioteca Node.js para EvolutionGO
4. **langchain/langchainjs** - Framework para IA com LLMs
5. **llama-index/llama-index** - RAG e agentes de IA
6. **vinta/awesome-langchain** - Repositório de exemplos LangChain
7. **notificame-hub** - Documentação das integrações

### Bibliotecas Úteis

- `whatsapp-web.js` - Controle do WhatsApp Web
- `express-whatsapp-api` - Wrapper para APIs
- `langchain` / `langgraph` - Frameworks de agentes AI
- `faiss` / `chromadb` - Banco de vetores para RAG
