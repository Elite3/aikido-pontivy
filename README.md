# Pontivy Aïkido — Site Web Officiel

Site vitrine statique et moderne pour le club **Pontivy Aïkido** (enseignement A.I.A.T.J. au Dojo de Kérantré à Pontivy).  
Architecture *Jamstack* ultra-légère, sans base de données, avec moteur d'édition visuelle en direct (*In-Context Live Editing*) connecté à l'API GitHub.

---

## 1. Architecture Technique

* **Frontend :** HTML5 sémantique, Tailwind CSS (via CDN), Leaflet.js (OpenStreetMap pour la cartographie locale).
* **Typographies :** *Cinzel* (titres et ambiance martiale) et *Inter* (corps de texte et lisibilité mobile).
* **Source de données :** Fichier unique `content/club.json` contenant l'intégralité des variables du site (textes, créneaux, tarifs, composition du bureau, coordonnées).
* **Moteur d'édition :** Édition directe *in situ* (`contenteditable="true"`) synchronisée via l'API REST de GitHub (`PUT /repos/{owner}/{repo}/contents/content/club.json`) ou téléchargeable en JSON local.
* **Hébergement recommandé :** Netlify, Cloudflare Pages ou GitHub Pages (déploiement continu automatisé à chaque commit).

---

## 2. Arborescence du Projet

```text
aikido-pontivy/
│
├── content/
│   └── club.json         # Données dynamiques du club (textes, horaires, tarifs, bureau)
│
├── images/
│   └── uploads/          # Stockage des médias et affiches de stages
│
├── admin/
│   └── index.html        # Point d'accès rapide avec redirection admin
│
├── index.html            # Application web complète (interface publique + moteur d'édition)
└── README.md             # Documentation technique et guide d'utilisation
```

---

## 3. Guide de Déploiement Initial (Administrateur)

### Étape 1 : Créer le dépôt GitHub

1. Créez un nouveau dépôt sur GitHub (public ou privé) nommé `aikido-pontivy`.
2. Poussez les fichiers du projet sur la branche `main`.

### Étape 2 : Configurer les identifiants dans `index.html`

Ouvrez le fichier `index.html` et mettez à jour les constantes JavaScript situées au début du script d'administration :

```javascript
const GITHUB_OWNER = "Elite3"; 
const GITHUB_REPO  = "aikido-pontivy";
const FILE_PATH    = "content/club.json";
```

### Étape 3 : Déployer sur Netlify (ou Cloudflare Pages)

1. Rendez-vous sur [Netlify](https://app.netlify.com/) et connectez votre compte GitHub.
2. Cliquez sur **Add new site** > **Import an existing project**.
3. Sélectionnez le dépôt `aikido-pontivy`.
4. Paramètres de compilation :
   * **Build command :** *(laisser vide)*
   * **Publish directory :** `.` *(ou laisser vide pour la racine)*
5. Cliquez sur **Deploy site**. Le site est en ligne en HTTPS sous une URL du type `https://aikido-pontivy.netlify.app`.

---

## 4. Génération du Jeton d'Administration (GitHub Token)

Pour permettre aux bénévoles de modifier le site directement depuis leur navigateur sans serveur backend :

1. Connectez-vous sur GitHub puis allez dans :
   **Settings** > **Developer Settings** > **Personal access tokens** > **Fine-grained tokens**.
2. Cliquez sur **Generate new token**.
3. Renseignez les paramètres :
   * **Token name :** `Editeur Site Aikido Pontivy`
   * **Expiration :** 1 an (ou *No expiration* selon votre politique)
   * **Repository access :** *Only select repositories* > choisir `aikido-pontivy`
   * **Permissions :** Déroulez *Repository permissions*, trouvez **Contents** et passez-le en **Access: Read and write**.
4. Cliquez sur **Generate token** et copiez la clé générée (commençant par `github_pat_` ou `ghp_`).
5. Transmettez ce jeton de manière sécurisée aux membres du bureau habilités.

---

## 5. Guide Bénévoles : Modifier le Site en Direct

Aucune connaissance technique n'est requise. La modification s'effectue directement sur la page web.

### 1. Activer le mode édition

* Rendez-vous sur le site web du club.
* Cliquez sur le lien **« ⚙️ Espace Administration »** dans le pied de page, ou utilisez le raccourci clavier :
  `Ctrl` + `Shift` + `E`
* Saisissez votre **Jeton d'accès** dans la fenêtre qui apparaît, puis validez.

### 2. Modifier les textes

* Lorsque le bandeau bleu **"Mode Éditeur Actif"** apparaît en bas de l'écran, les zones modifiables s'entourent de pointillés bleus.
* Cliquez sur n'importe quel texte pour le modifier directement à l'écran.

### 3. Publier ou Exporter

* **Publier directement :** Cliquez sur **« 💾 Publier les modifications »**. Le site se met à jour en 30 secondes.
* **Sauvegarde locale :** Cliquez sur **« 📥 Télécharger le JSON »** si vous préférez enregistrer le fichier `club.json` sur votre ordinateur sans utiliser de token GitHub.

---

## 6. Sécurité et Sauvegardes

* **Historique des versions :** Chaque publication génère un commit Git automatique sur GitHub. En cas d'erreur, l'administrateur peut restaurer une version antérieure depuis GitHub.
* **Stockage du Token :** Le token est conservé uniquement dans le `localStorage` de votre navigateur.
