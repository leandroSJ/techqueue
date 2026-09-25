# TechQueue — configuração do MVP seguro

O aplicativo continua sendo **uma única página (`index.html`)**.  
Os outros arquivos deste pacote são apenas configuração/documentação.

## Arquitetura

- GitHub Pages: hospeda o `index.html`
- Firebase Authentication: login individual
- Cloud Firestore: fila, estado em tempo real e histórico
- Firestore Security Rules: impede gravação de atendimento em nome de outro técnico

O nome mostrado no sistema **não é digitado nem selecionado pelo técnico**.  
Ele vem de `users/{uid}` no Firestore, ligado ao UID da conta autenticada.

## 1. Criar projeto Firebase

1. Acesse Firebase Console.
2. Crie um projeto.
3. Adicione um aplicativo Web.
4. Copie o objeto `firebaseConfig`.
5. Abra `index.html` e substitua os seis valores `COLE_AQUI`.

## 2. Authentication

Em **Authentication > Sign-in method**, habilite **Email/Password**.

Não existe botão "Criar conta" no TechQueue.

Crie manualmente uma conta para cada técnico:

- Leandro
- Ronald
- Italo
- Leonardo
- Javier

Depois de criar cada usuário, copie o `UID`.

### Domínio autorizado

Em **Authentication > Settings > Authorized domains**, adicione o domínio do GitHub Pages, por exemplo:

`leandrosj.github.io`

Se usar domínio próprio, adicione também esse domínio.

## 3. Firestore

Crie o Cloud Firestore.

Em **Rules**, substitua tudo pelo conteúdo de `firestore.rules` e publique.

Não deixe o banco em modo aberto/Test Mode para produção.

## 4. Cadastrar perfis aprovados

No Firestore, crie a coleção:

`users`

O ID de cada documento deve ser exatamente o UID da conta criada no Authentication.

### Leandro

Documento: `users/<UID_DO_LEANDRO>`

```text
name: "Leandro"
enabled: true
role: "admin" ou "tech"
isOpener: true
shiftStartMinutes: 480
```

### Ronald

```text
name: "Ronald"
enabled: true
role: "admin" ou "tech"
isOpener: true
shiftStartMinutes: 480
```

### Italo

```text
name: "Italo"
enabled: true
role: "tech"
isOpener: false
shiftStartMinutes: 540
```

### Leonardo

```text
name: "Leonardo"
enabled: true
role: "tech"
isOpener: false
shiftStartMinutes: 540
```

### Javier

```text
name: "Javier"
enabled: true
role: "tech"
isOpener: false
shiftStartMinutes: 540
```

`480 = 08:00` e `540 = 09:00`.

Escolha pelo menos uma conta como `role: "admin"` para poder usar a limpeza do histórico após 18:00.

## 5. Funcionamento do rodízio

### 08:00

Somente perfis com `isOpener: true` conseguem iniciar o dia.

Portanto, o primeiro técnico sempre será:

- Leandro; ou
- Ronald

O primeiro dos dois que entrar no sistema inicia a fila.

### 09:00

Italo, Leonardo e Javier passam a poder entrar automaticamente no rodízio quando estiverem logados.

### Atendi agora

Somente o técnico indicado como **próximo** fica com o botão habilitado.

Ao clicar:

1. abre campo `Cliente`;
2. registra o atendimento com o UID real do usuário autenticado;
3. técnico fica `Em atendimento`;
4. ele sai temporariamente da fila;
5. o próximo técnico aparece imediatamente para todos.

### Finalizei

O próprio técnico informa:

- Problema
- Solução

Ao finalizar:

1. o histórico é atualizado;
2. o técnico fica disponível novamente;
3. entra no final da fila.

## 6. Limpeza após 18:00

Somente perfil `admin` vê a função administrativa.

O botão só habilita depois das 18:00 no horário de Bahia.

As regras também restringem a exclusão do histórico para administrador na janela após 18:00.

## 7. GitHub Pages

Coloque `index.html` na raiz do repositório.

Depois:

1. GitHub > Settings
2. Pages
3. Deploy from a branch
4. `main`
5. `/ (root)`

O endereço normalmente ficará parecido com:

`https://SEU_USUARIO.github.io/NOME_DO_REPOSITORIO/`

## Segurança importante

O endereço do GitHub Pages pode ser descoberto. **Não trate o link como senha.**

A proteção do TechQueue é:

1. login Firebase;
2. usuário precisa existir em `users/{uid}`;
3. `enabled` precisa ser `true`;
4. o histórico usa `request.auth.uid`;
5. o navegador não pode criar ou editar perfis da coleção `users`.

Mesmo que alguém consiga criar uma conta Firebase por fora, sem um documento aprovado em `users/{uid}` essa conta não consegue ler nem gravar os dados do TechQueue.

### Limitação consciente deste MVP

As regras já impedem um técnico de registrar ou finalizar atendimento **em nome de outro técnico**.

Por ser um projeto 100% estático, os técnicos aprovados ainda têm permissão para atualizar o documento global que controla a fila. A interface só executa as transições corretas, mas uma pessoa autenticada e mal-intencionada poderia tentar manipular diretamente esse estado usando ferramentas de desenvolvedor/API.

Se o teste interno for aprovado e o sistema virar ferramenta oficial, a próxima evolução recomendada é mover as transições do rodízio para:

- Firebase Cloud Functions; ou
- backend Spring Boot.

Assim também protegemos no servidor quem pode alterar **a própria fila**, além da autoria dos atendimentos.
