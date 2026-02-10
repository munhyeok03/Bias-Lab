# Bias Market (Bias-Lab)

Bias Market is a deliberately vulnerable storefront demo built for security training and internal labs.
It looks and behaves like a modern commerce site, but it intentionally includes common web
vulnerability classes for controlled testing. **Do not deploy publicly.**

## Quick start (Docker)

```bash
docker build -t bias-lab:latest .
docker run --rm -p 8000:8000 bias-lab:latest
```

Open `http://localhost:8000`.

## Architecture

- **Backend**: Flask app in `app.py` with SQLite persistence.
- **Templates**: Jinja2 templates under `templates/`.
- **Static assets**: CSS, JS, and images under `static/`.
- **Storage**: SQLite DB stored in `data/` at runtime. Uploads go to `uploads/`.

## Directory layout

| Path | Purpose |
| --- | --- |
| `app.py` | Flask application, routes, and vulnerable behaviors. |
| `templates/` | HTML templates for all pages. |
| `static/` | CSS, JS, and imagery. |
| `static/images/` | Product and hero images. |
| `data/` | Runtime SQLite database (created on first run). |
| `uploads/` | Uploaded files (created on first run). |
| `public/` | Public documents served via the files viewer. |
| `secrets/` | Sample environment data (created on first run). |

## Vulnerability coverage (high-level)

Each category below is intentionally exposed in a realistic page or API surface.
This list is meant for training documentation only and avoids step-by-step exploitation.

| Category | Surface | Where it appears |
| --- | --- | --- |
| SQLi | Dynamic SQL composition | Product search and filter logic in `/products` and `/api/products/search`. |
| XSS | Unsanitized rendering | Search echo in `/search` and user-generated review content on `/product/<id>`. |
| CMDi | Shell execution | Delivery routing checks in `/diagnostic` and `/api/diagnostic`. |
| Path Traversal | File path handling | Document viewer in `/files` and `/api/file`. |
| Auth Bypass | Weak login logic | Legacy login flow in `/login` and `/api/login`. |
| IDOR | Insecure object access | Direct account and order references in `/account/<id>` and `/orders/<id>`. |
| SSRF | Server-side fetch | Partner preview in `/fetch` and `/api/fetch`. |
| CSRF | Missing anti-CSRF | Wallet transfer in `/transfer` and `/api/transfer`. |
| File Upload | Unrestricted upload | Seller upload flow in `/upload` and `/api/upload`. |
| Info Disclosure | Debug endpoints | System debug and config endpoints like `/.env`, `/.git/config`, and `/debug`. |

## Notes

- All vulnerabilities are intentional for lab usage.
- The UI contains no direct hints, but the behavior surfaces are present for testing.

