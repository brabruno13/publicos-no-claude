# Instalação (para o Claude Code seguir)

Você é o Claude Code instalando o "Públicos no Claude" no computador do usuário. Faça tudo você mesmo e só peça ao usuário o que só ele pode fazer (o token da Meta e confirmações). Explique cada pedido em português simples. Nunca mostre o token na tela depois que o usuário colar.

## O que precisa existir
1. Node.js 22.13 ou mais novo.
2. O servidor MCP byadsco/meta-ads-mcp (código aberto, licença MIT), versão v4.0.0, compilado numa pasta estável.
3. Um token de acesso da Meta com as permissões `ads_read` e `ads_management`.
4. A skill `publicos-no-claude` copiada pra pasta de skills do Claude.

## Passos

### 1. Node.js
Rode `node --version`. Se não existir ou for menor que 22.13, mostre ao usuário o comando de instalação do sistema dele (macOS: `brew install node`; Windows: instalador em nodejs.org; Linux: gerenciador de pacotes) e espere ele confirmar. Não instale nada que peça senha sem avisar.

### 2. Servidor MCP da Meta
```bash
test -d ~/publicos-no-claude/meta-ads-mcp || git clone --depth 1 --branch v4.0.0 https://github.com/byadsco/meta-ads-mcp.git ~/publicos-no-claude/meta-ads-mcp
cd ~/publicos-no-claude/meta-ads-mcp
npm install
npm run build
```

### 3. Token da Meta
Explique ao usuário e espere ele fazer:
1. Entre em developers.facebook.com/apps e crie um app do tipo Empresa (ou use um que já tenha).
2. Abra o Explorador da Graph API (developers.facebook.com/tools/explorer), escolha esse app e adicione as permissões `ads_read` e `ads_management`.
3. Clique em Gerar token de acesso e aprove.
4. Pra durar mais (cerca de 60 dias), abra a Ferramenta de token de acesso, encontre o token e clique em Estender token de acesso.
5. Cole o token aqui quando eu pedir.

Peça o token uma vez e registre o servidor sem ecoar o valor:
```bash
claude mcp add meta-ads --scope user -e META_ACCESS_TOKEN="$TOKEN" -- node ~/publicos-no-claude/meta-ads-mcp/dist/index.js --transport stdio
```

### 4. Skill
```bash
mkdir -p ~/.claude/skills
cp -r ./skills/publicos-no-claude ~/.claude/skills/
```
(rode de dentro da pasta deste repositório)

### 5. Teste
Peça ao usuário pra reiniciar o Claude Code. Depois rode, só leitura:
- liste as contas de anúncio dele;
- pesquise o interesse "Investimento imobiliário".

Se as duas coisas funcionarem, diga que está pronto e sugira o primeiro pedido: "Monta os públicos pro meu negócio".

## Atualizar o token
O token expira. Quando as ferramentas pararem com erro de autenticação, repita o passo 3 e rode `claude mcp remove meta-ads --scope user` antes do `claude mcp add`.

## Segurança
- O token dá acesso às suas contas de anúncio. Não compartilhe e não publique.
- Tudo o que a skill cria nasce pausado. Nada gasta verba até você ativar no Gerenciador.
