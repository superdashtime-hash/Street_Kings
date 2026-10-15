# Street Kings

A browser basketball game (3v3, 2v2, 1v1) with tournaments, rarities, codes, online play and optional cloud saves.
It is a single `index.html` with no build step, so it runs on GitHub Pages as is.

## Put it on GitHub Pages

1. Create a new GitHub repository and upload everything in this folder (`index.html`, `README.md`, `firestore.rules`, `.nojekyll`).
2. In the repository go to **Settings, Pages**. Under **Build and deployment** choose **Deploy from a branch**, pick the `main` branch and the `/ (root)` folder, then save.
3. After a minute your game is live at `https://YOUR-USERNAME.github.io/YOUR-REPO/`.

The game works right away with accounts that save in each player's own browser. Online play and codes need nothing else.

## Turn on cloud accounts (optional)

Cloud accounts let a player log in on any device and keep their coins, players, titles and codes. They use Firebase, which has a free plan.

1. Go to the Firebase console and create a project (Google Analytics is not needed).
2. **Build, Authentication, Get started**, then enable the **Email/Password** sign-in method. Players still log in with just a name and password. The game turns the name into a private placeholder email behind the scenes.
3. **Build, Firestore Database, Create database**. Choose production mode and any region.
4. In Firestore open the **Rules** tab, click inside the editor, press Ctrl+A (Cmd+A on Mac) to select the default rules, paste the contents of `firestore.rules` over them, and press **Publish**.
5. **Project settings, General, Your apps, Add app, Web** (the `</>` icon). Copy the `firebaseConfig` values.
6. Open `index.html`, find the block marked `CLOUD SAVES (optional)` near the top of the script, and fill it in:

   ```js
   const CLOUD={apiKey:'...',authDomain:'...',projectId:'...',appId:'...'};
   ```

7. Commit the change. When `apiKey` and `projectId` are filled in, the Account screen switches to cloud accounts automatically.

### Recommended hardening

- In the Google Cloud console, open **APIs and Services, Credentials**, edit the browser key Firebase created, and set **Website restrictions** to `https://YOUR-USERNAME.github.io/*`. The key is meant to be public, but this stops other sites from using it.
- If you use a custom domain, add it under Firebase **Authentication, Settings, Authorized domains**.

## Good to know

- Cloud passwords need at least 6 characters. There is no password reset, because players have no email on file.
- Saves are stored per player and the rules block everyone else from reading them. Players can still edit their own save, so treat coins and codes as fun progress, not as secure.
- Redeem codes live in the `CODES` list in `index.html` as scrambled values. To add one, tell whoever maintains the game, or generate a hash with the same `h53('sk|CODE')` function in the file.
- Online matches connect players directly by swapping two codes. No server is involved.
