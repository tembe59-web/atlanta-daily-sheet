# Katana Trading · Daily Delivery Sheet (Firebase)

Sistema web de gestão de entregas diárias com base de dados Firebase Firestore em tempo real.

## Funcionalidades

- **Dashboard** com métricas do mês (sheets, entregas, realizadas, caixas)
- **Criar / editar / eliminar** daily sheets
- **Registo de entregas** com estado (Realizada / Pendente / Falhada)
- **Registo de produtos** com código, descrição, cliente, caixas, unidades, HS Code
- **Histórico** com pesquisa por data, motorista ou rota
- **Impressão** A4 optimizada de cada sheet
- **Sincronização em tempo real** via Firebase Firestore (dados partilhados entre todos os dispositivos)
- **Indicador de ligação** na sidebar (Sincronizado / A ligar / Sem ligação)
- **Responsivo** — desktop, tablet e mobile

---

## PASSO 1 — Criar projecto Firebase

1. Aceda a **https://console.firebase.google.com**
2. Clique em **"Criar um projecto"** (ou seleccione um existente)
3. Dê um nome, ex: `katana-delivery`
4. Google Analytics: pode desactivar (opcional)
5. Clique em **"Criar projecto"**

---

## PASSO 2 — Configurar Firestore Database

1. No menu lateral do Firebase Console, vá a **Build → Firestore Database**
2. Clique em **"Criar base de dados"**
3. Seleccione **"Começar em modo de produção"**
4. Escolha a região: `eur3 (europe-west)` (ou a mais próxima)
5. Clique em **"Ativar"**

### Regras de segurança (Firestore Rules)
1. No Firestore, clique no separador **"Regras"**
2. Substitua o conteúdo pelo ficheiro `firestore.rules` incluído neste projecto:
```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /daily_sheets/{docId} {
      allow read, write: if true;
    }
  }
}
```
3. Clique em **"Publicar"**

---

## PASSO 3 — Registar a Web App e obter as credenciais

1. No Firebase Console, clique no ícone da engrenagem (⚙) → **"Definições do projecto"**
2. Na secção **"As suas aplicações"**, clique em **`</>`** (Web)
3. Dê um nome à app, ex: `katana-web`
4. **Não active** o Firebase Hosting por agora (vamos usar GitHub Pages)
5. Clique em **"Registar aplicação"**
6. Copie o bloco `firebaseConfig`:

```javascript
const firebaseConfig = {
  apiKey: "AIzaSy...",
  authDomain: "katana-delivery.firebaseapp.com",
  projectId: "katana-delivery",
  storageBucket: "katana-delivery.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abc123"
};
```

---

## PASSO 4 — Editar o ficheiro de configuração

Abra o ficheiro `js/firebase-config.js` e substitua os valores:

```javascript
const firebaseConfig = {
  apiKey:            "COLE_O_SEU_apiKey_AQUI",
  authDomain:        "COLE_O_SEU_authDomain_AQUI",
  projectId:         "COLE_O_SEU_projectId_AQUI",
  storageBucket:     "COLE_O_SEU_storageBucket_AQUI",
  messagingSenderId: "COLE_O_SEU_messagingSenderId_AQUI",
  appId:             "COLE_O_SEU_appId_AQUI"
};
```

---

## PASSO 5 — Publicar no GitHub Pages

### 5.1 — Criar repositório no GitHub

1. Aceda a **https://github.com** e faça login
2. Clique em **"New repository"**
3. Nome: `katana-daily-sheet`
4. Visibilidade: **Public**
5. Clique em **"Create repository"**

### 5.2 — Fazer upload dos ficheiros

**Opção A — Pelo browser (mais fácil):**
1. No repositório, clique em **"uploading an existing file"**
2. Arraste todos os ficheiros e pastas:
   - `index.html`
   - `css/style.css`
   - `js/firebase-config.js` ← já com as suas credenciais
   - `js/app.js`
   - `firestore.rules`
   - `README.md`
3. Escreva uma mensagem de commit, ex: `"Primeiro deploy — Katana Daily Sheet"`
4. Clique em **"Commit changes"**

**Opção B — Via Git:**
```bash
git init
git add .
git commit -m "Primeiro deploy — Katana Daily Sheet"
git branch -M main
git remote add origin https://github.com/SEU_UTILIZADOR/katana-daily-sheet.git
git push -u origin main
```

### 5.3 — Activar GitHub Pages

1. No repositório, vá a **Settings → Pages**
2. Em **Source**, seleccione **"Deploy from a branch"**
3. Branch: **main** / Pasta: **/ (root)**
4. Clique em **"Save"**
5. Aguarde 1-3 minutos. O link aparecerá:

```
https://SEU_UTILIZADOR.github.io/katana-daily-sheet/
```

---

## PASSO 6 — Autorizar o domínio no Firebase

Para que o Firebase aceite pedidos vindos do GitHub Pages:

1. No Firebase Console → **Authentication** (se não tiver, pode saltar)
   — **ou** —
   Vá a **Project Settings → General** e verifique se o domínio está autorizado.
2. Para Firestore sem autenticação (regras `allow read, write: if true`) não é necessário.
3. Se mais tarde activar autenticação, vá a **Authentication → Settings → Authorized domains** e adicione:
   ```
   SEU_UTILIZADOR.github.io
   ```

---

## Estrutura do projecto

```
katana-daily-sheet/
├── index.html              # Página principal
├── css/
│   └── style.css           # Estilos
├── js/
│   ├── firebase-config.js  # ← EDITE ESTE FICHEIRO com as suas credenciais
│   └── app.js              # Lógica da aplicação
├── firestore.rules         # Regras de segurança Firestore
└── README.md
```

---

## Notas de segurança

As regras actuais (`allow read, write: if true`) permitem acesso público. Para uso em produção com dados sensíveis, recomenda-se:

1. Activar o **Firebase Authentication** (email/password ou Google)
2. Actualizar as regras Firestore para exigir autenticação:
```
match /daily_sheets/{docId} {
  allow read, write: if request.auth != null;
}
```

---

## Tecnologias

- **Firebase Firestore** — base de dados em tempo real
- **HTML5 / CSS3 / JavaScript** (vanilla, sem frameworks)
- **DM Sans + DM Mono** — tipografia (Google Fonts)
- **Tabler Icons** — ícones
- **GitHub Pages** — hosting gratuito
