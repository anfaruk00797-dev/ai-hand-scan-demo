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

## Screen flow

`index.html` loops the 60 s tutorial video (`media/tutorial-60s.mp4`; until that file exists it falls back to `media/idle-premium.mp4`). The step bar at the top stays in the same place in every state. While the tutorial plays, it follows the video's chapters. During a real scan, it shows the step the user is actually on. The background is the only layer that changes.

```
home ─(hand seen ≥ 0.5 s)→ detecting → scan1 → flip → scan2 → analyzing → result ─(8 s)→ home
         hand gone > 5 s during a scan → home (warning shown after 1 s)
```

When it returns to Home, the video starts again from 0:00. A new scan won't start until the hand has been taken out at least once.

### Sending AI events to the screen

The AI program POSTs JSON to `http://127.0.0.1:8768/api/event` (one event or an array). The page receives them over SSE.

| Event | When to send |
|---|---|
| `{"type":"hand","present":true}` | Every frame or ~5–10×/s (this also acts as a heartbeat) |
| `{"type":"progress","value":0.0–1.0}` | During `scan1` / `scan2` |
| `{"type":"phase","phase":"flip"}` | First side done; also `"scan2"` and `"analyzing"` |
| `{"type":"result","clean":true,"score":96}` | Final result (`detail` optional text) |

```python
import requests
requests.post("http://127.0.0.1:8768/api/event", json={"type": "hand", "present": True}, timeout=0.2)
```

The page starts the scan by itself after the hand debounce, so no `scan1` phase is needed.

### Demo mode

If no AI event arrives for 3 s (or the URL has `?demo=1`), a simulator takes over. Tapping the screen counts as putting a hand in. Keys: `H` hand in/out, `C`/`U` force the next result to clean/not clean, `R` reset.

To show the inner camera behind the scan UI, add `?feed=<MJPEG URL>`.

## Included files

- `index.html` — HandClean Station screen (tutorial loop → live scan → result)
- `legacy.html` — the previous click-through video demo
- `media/` — the video and image assets used by the page
- `server.js` — a dependency-free local static file server

## รันบนเครื่อง

ติดตั้ง Node.js แล้วเปิด Terminal ในโฟลเดอร์นี้ จากนั้นรัน `node server.js` และเปิด <http://127.0.0.1:8768/> ในเบราว์เซอร์ แตะหน้าจอเพื่อเริ่มเล่น กด `Ctrl+C` ใน Terminal เพื่อหยุดเซิร์ฟเวอร์
