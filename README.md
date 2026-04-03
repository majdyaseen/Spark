# ✦ Spark — Speed Dating Website

A full-featured speed dating web app that runs entirely on **GitHub Pages** with **Firebase** as the backend (auth, database, file storage).

## Features
- 🔐 Login / Sign Up (email + Google)
- 👤 Rich profile editing (name, age, bio, location, looking for)
- 📸 Avatar + up to 4 extra photos (uploaded to Firebase Storage)
- 🔥 Browse & discover other members
- ♥️ Like system with **mutual match detection**
- 💞 Matches page
- 💬 Real-time chat between matched users
- 📱 Mobile responsive

---

## Setup (5 minutes)

### 1. Create a Firebase Project
1. Go to [console.firebase.google.com](https://console.firebase.google.com)
2. Click **Add project** → give it a name → Create
3. From the project dashboard, click **Web** (</>) to add a web app
4. Copy the `firebaseConfig` object shown

### 2. Enable Firebase Services
In your Firebase project sidebar:

**Authentication**
- Build → Authentication → Get started
- Enable **Email/Password** provider
- Enable **Google** provider (add your GitHub Pages domain to Authorized Domains later)

**Firestore Database**
- Build → Firestore Database → Create database
- Start in **test mode** (you can add security rules later)

**Storage**
- Build → Storage → Get started
- Start in **test mode**

### 3. Add Your Config
Open **both** files and replace the placeholder `firebaseConfig`:
- `index.html` (line ~175)
- `pages/app.html` (line ~320)

```js
const firebaseConfig = {
  apiKey: "AIza...",
  authDomain: "your-project.firebaseapp.com",
  projectId: "your-project-id",
  storageBucket: "your-project.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123:web:abc"
};
```

### 4. Deploy to GitHub Pages
1. Create a new GitHub repository
2. Upload all files (keep the folder structure):
   ```
   index.html
   pages/
     app.html
   README.md
   ```
3. Go to repo **Settings → Pages → Source: Deploy from branch → main → / (root)**
4. Your site will be live at `https://YOUR_USERNAME.github.io/REPO_NAME/`

### 5. Add GitHub Pages domain to Firebase Auth
- Firebase Console → Authentication → Settings → Authorized domains
- Add `YOUR_USERNAME.github.io`

---

## Firestore Security Rules (recommended for production)

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{userId} {
      allow read: if request.auth != null;
      allow write: if request.auth.uid == userId;
    }
    match /chats/{chatId} {
      allow read, write: if request.auth != null && 
        request.auth.uid in resource.data.users;
      match /messages/{msgId} {
        allow read, write: if request.auth != null;
      }
    }
    match /matches/{matchId} {
      allow read: if request.auth != null;
      allow write: if request.auth != null;
    }
  }
}
```

## Storage Security Rules

```
rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    match /avatars/{userId} {
      allow read: if request.auth != null;
      allow write: if request.auth.uid == userId;
    }
    match /photos/{userId}/{photo} {
      allow read: if request.auth != null;
      allow write: if request.auth.uid == userId;
    }
  }
}
```

---

## File Structure

```
spark/
├── index.html        ← Landing page + login/signup
├── pages/
│   └── app.html      ← Main app (browse, profile, matches, chat)
└── README.md
```

## Tech Stack
- **Frontend**: Vanilla HTML/CSS/JS — no build step needed
- **Auth**: Firebase Authentication (email + Google)
- **Database**: Cloud Firestore (real-time)
- **Storage**: Firebase Storage (photos)
- **Hosting**: GitHub Pages (free)
