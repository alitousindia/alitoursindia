# Testing Guide

Use these commands to validate the tour website quickly:

## 1) HTML Parse Check
```bash
python3 - <<'PY'
from html.parser import HTMLParser
from pathlib import Path
class P(HTMLParser):
    pass
P().feed(Path('index.html').read_text())
print('HTML parsed successfully')
PY
```

## 2) JavaScript Syntax Check (inline script extraction)
```bash
python3 - <<'PY'
from pathlib import Path
import re
html = Path('index.html').read_text()
scripts = re.findall(r'<script>([\s\S]*?)</script>', html)
Path('/tmp/alitours-inline.js').write_text('\n'.join(scripts))
print(f'Extracted {len(scripts)} inline script block(s)')
PY
node --check /tmp/alitours-inline.js
```

## 3) Browser Smoke Check
1. Run local server:
```bash
python3 -m http.server 4173 --bind 0.0.0.0 --directory /workspace/alitoursindia
```
2. Open in browser and verify:
   - package cards visible
   - package dropdown updates booking summary
   - booking submits with at least one monument selected
   - Pay buttons open Razorpay checkout (with valid key)

> Note: replace `YOUR_RAZORPAY_KEY` in `index.html` before real payment testing.
