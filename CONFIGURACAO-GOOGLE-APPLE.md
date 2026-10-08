# LiDire — configuração do Google Account e Apple ID

A versão VIGÉSIMO TERCEIRO adiciona a conexão real de contas Google e Apple ao bloco **Perfil → Conectividade**.

## Google Account

1. No Google Cloud Console, crie/configure um projeto.
2. Configure a tela de consentimento OAuth.
3. Crie um **OAuth 2.0 Client ID** do tipo **Web application**.
4. Cadastre o domínio/JavaScript origin onde a LiDire será acessada. Para o ambiente atual, use o domínio efetivamente publicado pela LiDire.
5. Guarde o Client ID.
6. No Cloudflare, configure:

```bash
npx wrangler secret put GOOGLE_CLIENT_ID
```

Cole o Client ID quando o Wrangler pedir.

A LiDire usa o Google Identity Services no navegador e envia o ID token ao Worker. O Worker verifica assinatura, emissor, audiência, validade e e-mail verificado antes de gravar a vinculação.

## Apple ID

Para Sign in with Apple na web, a Apple exige uma configuração no Apple Developer: App ID principal com Sign in with Apple, **Services ID**, domínio/retorno e uma **private key**. A documentação oficial informa que o Services ID é o identificador usado pelo site e que a URL de retorno precisa ser cadastrada exatamente. 

Configure estes valores no Cloudflare:

```bash
npx wrangler secret put APPLE_CLIENT_ID
npx wrangler secret put APPLE_TEAM_ID
npx wrangler secret put APPLE_KEY_ID
npx wrangler secret put APPLE_PRIVATE_KEY
```

Para `APPLE_PRIVATE_KEY`, cole o conteúdo completo do arquivo `.p8` fornecido pela Apple.

A URL de retorno usada pela versão é:

```text
https://SEU-DOMINIO/api/connections/apple/callback
```

Ela precisa ser cadastrada na configuração do Services ID na Apple.

## Importante

Sem esses identificadores/credenciais, os botões continuam aparecendo, mas a LiDire informa que a integração ainda não foi configurada. Isso é intencional: não há como autenticar contra Google ou Apple sem as credenciais do proprietário do aplicativo.

A implementação desta versão é de **vinculação de identidade à conta LiDire já autenticada**. Ela não substitui ainda o botão de login por senha da tela inicial.
