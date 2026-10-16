# Órbita

Painel pessoal para uma tela pequena na horizontal. HTML, CSS e JavaScript em módulos, sem etapa de build. Relógio, agenda e configurações; cor e emoji por item; edição, conclusão, progresso de metas; importação validada com prévia e exportação JSON. Sem dados de demonstração misturados aos seus planos. O fundo é preto absoluto (`#000000`) e a interface tem escala própria de 100% a 200%, salva por navegador.

## Executar

Requer Node.js 22 ou superior. Na pasta do projeto: `npm start`. Abra http://127.0.0.1:4173. Não abra index.html diretamente pelo explorador: módulos precisam de HTTP. Com spacedesk, abra o navegador do PC e mova a janela para a terceira tela. O servidor local escuta apenas no PC; não oferece acesso direto por outro aparelho na rede.

`npm test` verifica datas, importação, duplicatas e conteúdo inválido. `npm run check` verifica sintaxe. A aparência e o comportamento de navegador precisam ser conferidos na tela real.

## Firebase

O projeto dedicado é `orbita-nicolasneves`. O aplicativo Web está configurado em `dist/firebase-config.js`; essa configuração é pública por definição e não contém conta de serviço, senha ou chave administrativa. O Firestore padrão fica em São Paulo (`southamerica-east1`), com proteção contra exclusão acidental.

A versão publicada no Firebase Hosting está em https://orbita-nicolasneves.web.app. Para publicar uma atualização: `npx firebase-tools deploy --only hosting --project orbita-nicolasneves`.

E-mail/senha e Google estão habilitados pelo bloco `auth` de `firebase.json`; os domínios do Firebase Hosting e `nicolasneves.com.br` estão autorizados. Os itens ficam em `users/{uid}/items/{id}`; as regras publicadas limitam leitura, escrita e exclusão ao próprio usuário e validam todos os campos aceitos pelo aplicativo.

Sem configuração, o app funciona em modo local e informa isso. Login não é simulado. Ao entrar, o app troca para os dados da conta; itens locais ficam separados, com botão para copiá-los à conta. Ao sair, retorna ao modo local. Preferências visuais ficam neste navegador. Escritas na nuvem só são informadas como salvas após confirmação do Firestore. Não há fila offline implementada.

## GitHub e nicolasneves.com.br/orbita

O código está em `https://github.com/nicolasnevesdg/orbita`. O fluxo `.github/workflows/pages.yml` publica a pasta `dist` em `https://nicolasnevesdg.github.io/orbita/`. A mesma pasta também foi acrescentada como `orbita/` no repositório do portfólio, deixando o endereço principal ativo em `https://nicolasneves.com.br/orbita/` sem substituir a página inicial. O remoto Git `portfolio` aponta para esse segundo repositório para facilitar futuras atualizações. A configuração do Firebase incluída no repositório é pública e segura para o navegador; nunca adicione conta de serviço ou tokens.

## Importar planos preparados no ChatGPT

Exporte ou use “Modelo JSON” em Configurações. Formato: `{ "version": 1, "items": [...] }`. Cada item tem título, data YYYY-MM-DD, tipo (`task`, `event`, `goal`, `reminder`) e categoria (`personal`, `work`, `study`, `health`). Campos opcionais: id estável, time/endTime HH:mm, emoji (vazio remove), color #RRGGBB, notes, priority low/normal/high, progress 0–100 e done booleano. Não coloque dados de conta ou tokens no JSON. Importações adicionam itens e ignoram IDs já presentes; não substituem nem apagam dados. Até 2.000 itens e 5 MB. Operações de nuvem são feitas em lotes: uma falha parcial pode ser retomada importando o mesmo arquivo.

## Decisões e limites

- Fundo preto absoluto e acento verde suave. A interface usa DM Sans; o relógio é desenhado em sete segmentos chanfrados diretamente em CSS, para permanecer fiel e nítido em qualquer resolução.
- A escala inicial é 125%. Em Configurações → Escala da interface, escolha de 100% a 200%. Isso corrige monitores pequenos de alta resolução, como o celular usado pelo spacedesk, sem depender do zoom do navegador.
- Navegação e brilho nas laterais aparecem com mouse ou foco de teclado. Em toque ficam visíveis. F ou botão ativa tela cheia quando suportado; F11 também funciona no navegador do PC.
- Brilho é uma camada preta, não altera o brilho físico do dispositivo.
- “Hoje” volta à data atual. Retorno automático depende de inatividade, é configurável e não interrompe formulários. A data e hora seguem o sistema do navegador; datas são armazenadas sem conversão UTC.
- Eventos são limitados a um dia; repetição, subtarefas, categorias personalizadas, metas de longo prazo e notificações de sistema ainda não foram implementadas. Lembretes são itens visuais, sem alerta com a página fechada.
- O seletor oferece emojis frequentes e campo livre: use Win + ponto para o catálogo completo do sistema, ou deixe vazio.
- GugetFin e acesso direto do ChatGPT não estão conectados. Hoje é possível pedir um JSON e importar. Uma futura API autenticada deve limitar acesso à sua conta, permitir revogação e registrar alterações; nunca colocar credenciais administrativas no front-end.
- O Firebase está provisionado. A criação de uma conta real não faz parte dos testes automatizados; antes de depender exclusivamente da nuvem, faça um primeiro login e mantenha uma exportação JSON como backup.

## Organização

`dist/index.html`: estrutura; `styles.css`: visual; `app.js`: interface; `model.js`: dados e validação; `cloud.js`: autenticação e Firestore; `firebase-config.js`: configuração pública; `tests/`: validações; `firestore.rules`: isolamento por usuário; `server.js`: preview local.
