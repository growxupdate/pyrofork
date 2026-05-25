# Pyrofork Lite Stable Patch 2.3.69.2

Base: pyrofork-main.

Goal: keep Pyrofork trusted base, make it closer to Kurigram-style long-running behavior without adding hard queue caps or heavy custom watchdog logic.

Changes:
- Version bumped to 2.3.69.2.
- Added `lite_updates=True` to `Client` by default.
- In lite mode, dispatcher parses only common bot/user updates:
  - new messages
  - edited messages
  - deleted messages
  - callback queries
  - user status
  - inline queries
  - polls
  - chosen inline results
  - chat member updates
  - join requests
  - shipping/pre-checkout queries
- In lite mode, these extra high-noise/newer update parsers are skipped:
  - business messages/connects
  - stories
  - message reaction updates/counts
  - purchased paid media
- No `max_update_queue_size` was added. Queue behavior remains natural/unbounded like upstream/Kurigram style.
- Added `download_media(..., no_temp=True)` support.
  - Default remains safe `.temp` + rename.
  - `no_temp=True` writes directly to final path.
- Media session/CDN code was left on the pyrofork base path; no risky replacement/backdoor-prone code was imported.

Usage:
```python
app = Client("bot", ..., lite_updates=True)   # default
```

To restore all pyrofork update parsers:
```python
app = Client("bot", ..., lite_updates=False)
```

For no-temp download:
```python
path = await app.download_media(message, no_temp=True)
```
