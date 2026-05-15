# Ariba Launcher POC

Página intermediária estática para validar a abertura do Intelligent Buy a partir de um modal/iframe da SAP Ariba.

## Tecnologia

HTML, CSS e JavaScript puros. Não depende do app Next principal e pode ser publicada como site estático no Vercel ou em host equivalente.

## Rotas configuradas

A configuração fica em `launcher.config.js`:

```js
window.ARIBA_LAUNCHER_CONFIG = {
  appBaseUrl: 'https://uintelligentbuy-hml.stratesys.io',
  mapPath: '/admin/maps',
  summaryPath: '/admin/review/quotation',
};
```

- `mapPath`: alias existente do app principal que resolve o Mapa Comparativo usando `projectId` e `eventId`.
- `summaryPath`: alias existente do app principal que resolve o Resumo Executivo premiado usando `projectId` e `eventId`.

Todos os query params recebidos pelo launcher são preservados na URL final, inclusive parâmetros desconhecidos.

## Como rodar localmente

Na raiz do repositório:

```bash
npx serve pocs/ariba-launcher
```

Depois abra:

```text
http://localhost:3000/ariba-launcher?realm=744862388-T&eventId=Doc2200752930&projectId=WS2200752923
```

Se o `serve` escolher outra porta, ajuste a URL conforme exibido no terminal.

## Como publicar no Vercel

1. Crie um novo projeto no Vercel apontando para este repositório.
2. Configure o diretório raiz do projeto como `pocs/ariba-launcher`.
3. Use build command vazio ou nenhum build command.
4. Use output/public directory vazio ou `.` conforme a UI do Vercel solicitar.
5. Publique e configure na Ariba a URL publicada com o path `/ariba-launcher`.

Exemplo:

```text
https://seu-launcher.vercel.app/ariba-launcher?realm=...&eventId=...&projectId=...
```

## Testes manuais fora da Ariba

1. Rodar o site estático localmente.
2. Abrir `/ariba-launcher?realm=744862388-T&eventId=Doc2200752930&projectId=WS2200752923`.
3. Clicar em `Abrir Mapa` e validar se abre `https://uintelligentbuy-hml.stratesys.io/admin/maps` preservando os parâmetros.
4. Clicar em `Abrir Resumo` e validar se abre `https://uintelligentbuy-hml.stratesys.io/admin/review/quotation` preservando os parâmetros.
5. Simular bloqueio de popup e validar se aparece o fallback com link manual e botão de copiar.
6. Testar em viewport pequeno semelhante ao modal da Ariba.

## Testes manuais dentro da Ariba

1. Publicar a POC em Vercel ou host estático.
2. Configurar temporariamente a URL do launcher como link externo na Ariba.
3. Clicar no link dentro da Ariba.
4. Confirmar que a página carrega no modal/iframe.
5. Confirmar que os query params chegam corretamente.
6. Clicar em `Abrir Mapa` e validar abertura em nova aba.
7. Clicar em `Abrir Resumo` e validar abertura em nova aba.
8. Se o popup for bloqueado, validar o fallback manual.

## Pendências de validação

- Confirmar com o líder técnico se `/admin/review/quotation` é o alias final esperado para Resumo Executivo na integração Ariba.
- Confirmar se o sandbox/modal da Ariba permite `window.open` por clique do usuário.
- Confirmar se a configuração final da Ariba enviará sempre `realm`, `eventId` e `projectId`.
