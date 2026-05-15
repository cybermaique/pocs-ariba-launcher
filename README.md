# Intelligent Buy Ariba Launcher

POC estática de launcher para SAP Ariba. A página é carregada em um modal/iframe da Ariba e abre o Intelligent Buy em uma nova aba, preservando os query params recebidos.

## Como funciona

- Recebe os parâmetros enviados pela Ariba na URL do launcher.
- Mantém esses parâmetros ao abrir `Abrir Mapa` ou `Abrir Resumo`.
- Usa fallback manual quando o navegador ou o iframe bloqueia a abertura automática da nova aba.
- Não depende do app Next principal, backend, autenticação ou headers específicos.

## Configuração

Os destinos ficam centralizados em `launcher.config.js`:

```js
window.ARIBA_LAUNCHER_CONFIG = {
  appBaseUrl: 'https://uintelligentbuy-hml.stratesys.io',
  mapPath: '/admin/maps',
  summaryPath: '/admin/review/quotation',
};
```

- `appBaseUrl`: URL base do Intelligent Buy.
- `mapPath`: rota usada para abrir o Mapa.
- `summaryPath`: rota usada para abrir o Resumo.

O valor atual de `summaryPath` deve ser validado no ambiente Intelligent Buy/HML e no fluxo real da Ariba antes da aprovação final.

## Como rodar localmente

Na raiz deste projeto:

```bash
npx serve .
```

Depois abra a URL exibida pelo `serve`, por exemplo:

```text
http://localhost:3000
```

## Como testar com query params

Abra o launcher incluindo os parâmetros simulados da Ariba:

```text
http://localhost:3000?realm=744862388-T&eventId=Doc2200752930&projectId=WS2200752923
```

Valide:

1. `Abrir Mapa` abre `https://uintelligentbuy-hml.stratesys.io/admin/maps` preservando os parâmetros.
2. `Abrir Resumo` abre `https://uintelligentbuy-hml.stratesys.io/admin/review/quotation` preservando os parâmetros.
3. O fallback exibe `Abrir link manualmente` e `Copiar link` caso a nova aba seja bloqueada.
4. `launcher.config.js` carrega sem erro 404 no console.

## Deploy na Vercel

Configure o projeto da Vercel apontando para esta pasta como raiz do projeto. Não é necessário build.

Sugestão:

- Build command: vazio.
- Output directory: vazio ou `.` conforme a UI da Vercel solicitar.
- Arquivo de configuração: `launcher.config.js` deve ficar publicado junto com `index.html`.

## Validação dentro da Ariba

O teste final precisa ser feito dentro da SAP Ariba para confirmar:

1. Se o modal/iframe permite `window.open` em uma ação de clique do usuário.
2. Se os query params chegam corretamente ao launcher.
3. Se o Intelligent Buy recebe os mesmos parâmetros ao abrir Mapa ou Resumo.
4. Se `/admin/review/quotation` é a rota final correta para o Resumo no ambiente HML/Ariba.
