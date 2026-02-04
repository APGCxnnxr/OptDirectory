# Offline Game Launcher

This repository contains a single-file offline game launcher (launcher.html) and an example game (example-game.html). It is designed to be used on Chromium-based devices (Chromebook/Chrome) and works offline.

Features
- Built-in example games (6 tiles).
- Add a folder of HTML games via the File System Access API (Chromium). The launcher will auto-discover `.html` files and show them as tiles.
- Import single HTML files (fallback if folder access not available). Imported files persist in IndexedDB.
- Per-game save/load API via `postMessage` (saves stored in localStorage).
- Local account system stored in localStorage. Coins accrue automatically while playing (approx. 1 coin every 5 minutes).
- Fullscreen and Back/Home functionality.
- Games can access account info via postMessage.

How to use
1. Download the repository files and open `launcher.html` in Chrome (Chromebook).
2. To auto-discover games in a folder:
   - Click "Add folder" and select a folder containing `.html` games. Grant permission when prompted.
   - The launcher will list any `.html` files in that folder.
   - If you want automatic updates, enable "Auto-scan" (polls the folder every 15s).
3. To import a single HTML game file:
   - Click "Import file" and choose an HTML file. It will be stored locally in your browser.
4. Account:
   - Register or login from the header. Coins are shown in the header.
   - While a game is open, coins accumulate every 5 minutes of play and are saved locally.
5. Games can use the following `postMessage` protocol to save/load and access account info:
   - Save: `parent.postMessage({ type: 'launcher.save', slot: 'slotName', data: {...} }, '*')`
   - Load: `parent.postMessage({ type: 'launcher.load', slot: 'slotName', requestId: 'id' }, '*')`
   - List saves: `parent.postMessage({ type: 'launcher.listSaves', requestId: 'id' }, '*')`
   - Account get: `parent.postMessage({ type: 'account.get' }, '*')`
   - Account add coins: `parent.postMessage({ type: 'account.add', amount: 5 }, '*')`
   - Spend coins: `parent.postMessage({ type: 'account.spend', amount: X, requestId: 'id' }, '*')`

Make it a GitHub repo
1. Create a new repository on GitHub (or use the command line):
   - git init
   - git add launcher.html example-game.html README.md
   - git commit -m "Add offline launcher"
   - git branch -M main
   - git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO.git
   - git push -u origin main

Notes and caveats
- Folder selection relies on the File System Access API. This works in Chromium-based browsers (Chrome on Chromebook). If unsupported, use "Import file" to add games individually.
- If an imported or folder game uses relative assets (images, JS files), the launcher may not serve those assets correctly unless the game is a single self-contained HTML file or you import all supporting files and rewrite paths — for full folder support you'd need a small local HTTP server (recommended for complex games).
- Account data and saves are stored locally in the browser; they are not synced.
- If you want, I can:
  - Provide a ZIP of these files.
  - Show exact commands to create the GitHub repo and push files.
  - Add an Export/Import saves UI.
  - Add multi-slot save UI and thumbnails.

License
- Add a license file as needed (MIT recommended for public repos).
