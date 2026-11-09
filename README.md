# AI Hand Scan

Interactive hand-scan demo built with a single HTML page and local video assets.

## Run locally

1. Install [Node.js](https://nodejs.org/) (no extra packages are needed).
2. Open a terminal in this folder.
3. Run:

   ```sh
   node server.js
   ```

4. Open <http://127.0.0.1:8768/> and click the screen to begin.

The small Node.js server supports video byte-range requests so the clips can seek and play smoothly. Press `Ctrl+C` in the terminal to stop it.

## Included files

- `index.html` — the interactive page
- `media/` — the video and image assets used by the page
- `server.js` — a dependency-free local static file server

## รันบนเครื่อง

ติดตั้ง Node.js แล้วเปิด Terminal ในโฟลเดอร์นี้ จากนั้นรัน `node server.js` และเปิด <http://127.0.0.1:8768/> ในเบราว์เซอร์ แตะหน้าจอเพื่อเริ่มเล่น กด `Ctrl+C` ใน Terminal เพื่อหยุดเซิร์ฟเวอร์
