# WhatsApp API com Baileys

Uma API RESTful completa para gerenciar instâncias do WhatsApp usando a biblioteca @whiskeysockets/baileys. Esta aplicação permite criar múltiplas instâncias, gerar QR Codes, fazer pareamento, enviar mensagens e muito mais.

## Características

- 🚀 **Múltiplas Instâncias**: Gerencie várias conexões WhatsApp simultaneamente
- 📱 **QR Code**: Gere QR Codes para autenticação
- 🔗 **Pareamento**: Use códigos de pareamento como alternativa ao QR Code
- 💬 **Envio de Mensagens**: Texto, mídia, localização e contatos
- 🔔 **Webhooks**: Receba eventos em tempo real
- 📚 **Documentação Swagger**: Interface visual para testar endpoints
- 🛡️ **Segurança**: Rate limiting, CORS e validações
- 📁 **Upload de Arquivos**: Suporte para envio de mídias

## Instalação

### Pré-requisitos

- Node.js 18+ 
- npm ou yarn

### Passos

1. Clone o repositório:
```bash
git clone <url-do-repositorio>
cd whatsapp-api
```

2. Instale as dependências:
```bash
npm install
```

3. Configure as variáveis de ambiente:
```bash
cp .env.example .env
# Edite o arquivo .env conforme necessário
```

4. Inicie a aplicação:
```bash
# Desenvolvimento
npm run dev

# Produção
npm start
```

## Uso

### Acessar a Documentação

Após iniciar a aplicação, acesse:
- **Documentação Swagger**: http://localhost:3000/api-docs
- **Health Check**: http://localhost:3000/health
- **API Base**: http://localhost:3000/

### Fluxo Básico

1. **Criar uma Instância**:
```bash
POST /api/v1/instances
{
  "name": "Minha Instância",
  "webhook": "https://meusite.com/webhook" (opcional)
}
```

2. **Conectar a Instância**:
```bash
POST /api/v1/instances/{instanceId}/connect
```

3. **Obter QR Code**:
```bash
GET /api/v1/instances/{instanceId}/qrcode
```

4. **Ou usar Código de Pareamento**:
```bash
POST /api/v1/instances/{instanceId}/pairing-code
{
  "phone": "5511999999999"
}
```

5. **Enviar Mensagem**:
```bash
POST /api/v1/messages/text
{
  "instanceId": "uuid-da-instancia",
  "to": "5511999999999",
  "message": "Olá! Como você está?"
}
```

## Endpoints Principais

### Instâncias

- `GET /api/v1/instances` - Listar instâncias
- `POST /api/v1/instances` - Criar instância
- `GET /api/v1/instances/{id}` - Obter instância
- `DELETE /api/v1/instances/{id}` - Deletar instância
- `POST /api/v1/instances/{id}/connect` - Conectar
- `POST /api/v1/instances/{id}/disconnect` - Desconectar
- `GET /api/v1/instances/{id}/qrcode` - Obter QR Code
- `POST /api/v1/instances/{id}/pairing-code` - Gerar código de pareamento
- `GET /api/v1/instances/{id}/status` - Status da conexão

### Mensagens

- `POST /api/v1/messages/text` - Enviar texto
- `POST /api/v1/messages/media` - Enviar mídia
- `POST /api/v1/messages/location` - Enviar localização
- `POST /api/v1/messages/contact` - Enviar contato
- `GET /api/v1/messages/{instanceId}/history` - Histórico

### Webhooks

- `POST /api/v1/webhooks` - Configurar webhook
- `GET /api/v1/webhooks/{instanceId}` - Obter configuração
- `DELETE /api/v1/webhooks/{instanceId}` - Remover webhook
- `POST /api/v1/webhooks/test` - Testar webhook

## Webhooks

Os webhooks permitem receber eventos em tempo real. Configure uma URL para receber:

### Eventos Disponíveis

- `message` - Nova mensagem recebida
- `connection` - Mudanças na conexão
- `qr` - Novo QR Code gerado
- `pairing_code` - Código de pareamento gerado
- `connected` - Instância conectada
- `disconnected` - Instância desconectada
- `message_update` - Atualização de mensagem
- `presence` - Mudança de presença

### Formato do Payload

```json
{
  "event": "message",
  "instanceId": "uuid-da-instancia",
  "data": {
    "message": {...},
    "from": "5511999999999@s.whatsapp.net",
    "timestamp": 1234567890
  },
  "timestamp": "2023-12-01T10:00:00.000Z"
}
```

## Estrutura do Projeto

```
whatsapp-api/
├── src/
│   ├── controllers/     # Controladores da API
│   ├── routes/         # Definições de rotas
│   ├── services/       # Lógica de negócio
│   ├── middleware/     # Middlewares customizados
│   └── utils/          # Utilitários
├── sessions/           # Dados de sessão do WhatsApp
├── uploads/           # Arquivos enviados
├── .env              # Variáveis de ambiente
└── package.json      # Dependências e scripts
```

## Configuração

### Variáveis de Ambiente

```env
# Servidor
PORT=3000
NODE_ENV=development

# API
API_PREFIX=/api/v1

# Rate Limiting
RATE_LIMIT_WINDOW_MS=900000
RATE_LIMIT_MAX_REQUESTS=100

# Diretórios
SESSIONS_DIR=./sessions
UPLOADS_DIR=./uploads

# Webhook (opcional)
WEBHOOK_URL=
WEBHOOK_SECRET=

# Log
LOG_LEVEL=info
```

## Segurança

- **Rate Limiting**: Limite de requisições por IP
- **CORS**: Configurado para aceitar requisições de qualquer origem
- **Helmet**: Headers de segurança
- **Validação**: Validação de entrada em todos os endpoints
- **Sanitização**: Limpeza de dados de entrada

## Troubleshooting

### Problemas Comuns

1. **QR Code não aparece**:
   - Verifique se a instância está conectada
   - Tente desconectar e conectar novamente

2. **Mensagens não são enviadas**:
   - Verifique se a instância está com status "connected"
   - Confirme o formato do número (internacional)

3. **Webhook não funciona**:
   - Verifique se a URL está acessível
   - Teste o webhook usando o endpoint de teste

4. **Instância desconecta frequentemente**:
   - Verifique a conexão com a internet
   - Evite usar a mesma conta em múltiplos dispositivos

### Logs

Os logs são exibidos no console. Para debug, defina `LOG_LEVEL=debug` no arquivo `.env`.

## Contribuição

1. Fork o projeto
2. Crie uma branch para sua feature
3. Commit suas mudanças
4. Push para a branch
5. Abra um Pull Request

## Licença

Este projeto está sob a licença ISC.

## Aviso Legal

Este projeto é apenas para fins educacionais e de desenvolvimento. Use com responsabilidade e respeite os termos de serviço do WhatsApp.

