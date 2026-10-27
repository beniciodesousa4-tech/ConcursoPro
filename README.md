# ConcursoPro — versão inicial

Aplicativo pessoal responsivo para web e Android (instalável como PWA), com biblioteca de PDFs na nuvem, leitor, questões, histórico de desempenho, flashcards e cronograma.

## O que funciona nesta versão
- Interface responsiva azul-escuro/preto.
- Adicionar PDFs por concurso, armazenar no Firebase Storage e abrir no leitor.
- Extração de texto de PDFs pesquisáveis via PDF.js no navegador.
- Gerador simples de perguntas de revisão a partir de frases extraídas (não é IA e pode produzir perguntas imperfeitas).
- Cadastrar questões próprias de múltipla escolha, responder, corrigir e guardar resultados.
- Criar e revisar flashcards; marcar cartão como conhecido ou para rever.
- Criar tarefas do cronograma, concluir e acompanhar pendências.
- Sincronização em nuvem via Firestore/Storage quando Firebase está configurado.

## Requisitos
- Conta Google para criar projeto no Firebase.
- Um computador para configurar e publicar (ou serviço de hospedagem que aceite arquivos estáticos).
- Conexão à internet para nuvem e bibliotecas externas.

## 1. Configurar Firebase (gratuito dentro dos limites do plano)
1. Entre em https://console.firebase.google.com/ e crie um projeto.
2. Em Configurações do projeto > Geral, crie/registre um app Web e copie a configuração `firebaseConfig`.
3. Em Authentication > Sign-in method, habilite **Anonymous** para esta versão inicial.
4. Crie um Firestore Database.
5. Em Storage, habilite o Firebase Storage.
6. Em Firestore Rules, publique o conteúdo de `firebase.rules.firestore`.
7. Em Storage Rules, publique o conteúdo de `firebase.rules.storage`.
8. Hospede o app por HTTPS (por exemplo, Firebase Hosting). Abra Configuração dentro do app, cole o JSON da configuração e toque em Salvar e Conectar.

**Importante:** regras protegem os dados por UID, mas a autenticação anônima não é uma identidade recuperável entre navegadores/dispositivos. Se limpar os dados do navegador, pode perder acesso à conta anônima e aos seus documentos. Para uso duradouro em vários dispositivos, configure login por e-mail/Google e migração de conta antes de guardar materiais importantes.

## 2. Publicar com Firebase Hosting
1. Instale Node.js e Firebase CLI: `npm install -g firebase-tools`
2. No terminal, faça login: `firebase login`
3. Extraia esta pasta e entre nela: `cd ConcursoPro`
4. Inicialize Hosting: `firebase init hosting` e selecione seu projeto. Use `public` como diretório público (ou copie os arquivos para a pasta escolhida); configure como SPA: **No** é suficiente para esta versão.
5. Se usar `public`, copie `index.html`, `styles.css`, `app.js`, `manifest.json`, `sw.js` e `icon.svg` para `public/`.
6. Execute `firebase deploy`.
7. Abra a URL HTTPS gerada.

Alternativa: hospede os arquivos estáticos em qualquer provedor HTTPS. O recurso PWA/instalação exige HTTPS (localhost também serve para teste).

## 3. Instalar no Android
1. Abra a URL HTTPS no Chrome do Android.
2. Menu ⋮ > **Instalar app** ou **Adicionar à tela inicial**.
3. Confirme. O app ficará disponível como atalho/app independente.

A mesma URL funciona no computador. O PWA não é um APK publicado na Play Store, mas é instalável no Android.

## 4. Segurança e limitações
- Nunca publique regras abertas como `allow read, write: if true`.
- A configuração web do Firebase não é uma senha; as regras de segurança são obrigatórias.
- O MVP usa autenticação anônima e salva a configuração localmente no navegador. Não inclua segredos privados de servidor.
- PDFs escaneados como imagem podem não ter texto para extração; precisam de OCR.
- O gerador de perguntas é heurístico. Confira questões e gabaritos. Ele não garante que perguntas de concursos oficiais sejam reproduzidas corretamente.
- Não há integração com um modelo de IA nesta versão; isso exige backend seguro e uma chave de API protegida no servidor.
- Os arquivos `firebase.rules.*` são regras de exemplo e devem ser revisados para seu projeto antes de produção.

## 5. Executar localmente para testar
Abra a pasta em um terminal e rode:
`python -m http.server 8000`
Depois acesse `http://localhost:8000`. Para usar Firebase, registre o domínio local nas configurações do Authentication quando necessário. O modo local funciona sem Firebase com dados temporários do navegador; nuvem só funciona depois da configuração.
