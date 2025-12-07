
Wrestling Jeopardy with Remote Buzzers
=====================================

What's included:
- game.html     -> Streaming board to embed in EvMux
- control.html  -> Host control panel (accept buzzes, scoreboard)
- admin.html    -> Edit categories/questions; broadcast to control/game via BroadcastChannel
- buzzer.html   -> Player buzzer page (players manually enter their name)
- data.json     -> Default dataset
- firebase-config.example.js -> Example file; you must create firebase-config.js with your Firebase config
- README.txt    -> This file

Remote buzzer overview
----------------------
- Remote players open buzzer.html on their phones and manually enter their player/team name.
- When they press BUZZ, an entry is pushed to Firebase Realtime Database (/buzzes).
- The host's control.html listens for new buzzes and atomically sets /currentBuzz to the first buzz using a transaction.
- control.html displays who buzzed first. Click "Assign Question to Buzzing Team" to give them control/score.
- Reset Buzzers clears /buzzes and /currentBuzz in the DB.

Firebase setup (free)
---------------------
1. Go to https://console.firebase.google.com/
2. Create a new project (name it e.g., wrestling-jeopardy)
3. In the project, go to "Realtime Database" and create a database in your preferred location.
4. Set the database rules temporarily to open for testing (IMPORTANT: this is insecure; lock down in production):
{
  "rules": {
    ".read": true,
    ".write": true
  }
}
5. Go to Project Settings -> SDK Setup and Config. Copy the Firebase config object.
6. Create a file named firebase-config.js in the project root and paste:
   const FIREBASE_CONFIG = { ... your config ... };
7. Save and deploy (see GitHub Pages steps below).

GitHub Pages deployment (step-by-step)
-------------------------------------
1. Create a new GitHub repo (e.g., wrestling-jeopardy). Sign in to GitHub.
2. On your repo page, click "Add file" -> "Upload files". Upload all files from this package.
   - IMPORTANT: Before uploading, create firebase-config.js locally (do NOT commit sensitive keys publicly if you want DB security). For testing, it's okay temporarily.
3. Commit the files to the main branch.
4. Go to Settings -> Pages. Under "Source" choose branch: main, folder: / (root). Save.
5. Wait ~30 seconds. Your site will be at:
   https://<your-username>.github.io/<repo-name>/
6. Test pages:
   - Game board: https://<your-username>.github.io/<repo-name>/game.html
   - Control: https://<your-username>.github.io/<repo-name>/control.html
   - Admin: https://<your-username>.github.io/<repo-name>/admin.html
   - Buzzer for players: https://<your-username>.github.io/<repo-name>/buzzer.html

EvMux setup
-----------
1. Use the game.html URL above as a Web Page / URL widget in EvMux. Resize to show the full board.
2. Open control.html in a separate browser tab (host machine) to accept buzzers and reveal questions.
3. Share buzzer.html link with players so they can join and press BUZZ remotely.

Notes & security
----------------
- For production, restrict your Realtime Database rules (use Firebase Authentication or whitelist host IPs).
- If you do not want to include firebase-config.js in the public repo, host firebase-config.js elsewhere and update the HTML to point to it, or use GitHub Pages private repo with Actions to inject the config (advanced).
- BroadcastChannel works only for pages on the same origin. If you host game/control/admin on GitHub Pages under the same repo, BroadcastChannel will work between tabs on the same origin.

If you want, I can:
- Walk you through creating the Firebase project step-by-step and provide the exact config insertion snippet.
- Deploy the repo for you if you share the repo name and confirm you want me to prepare a commit (I cannot push directly to your GitHub without credentials).

