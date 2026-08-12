# AURA — Vitrina de Agentes IA (BMOPS Consulting)

Landing premium con **4 productos IA funcionales** en una sola página, construida en HTML/CSS/JS puro (sin frameworks).

## 🎬 En vivo: https://aura.bmops.tech

## ✨ Características

### Efectos premium (JS vanilla, sin librerías)
- **Text-scramble** — el título se "descodifica" al cargar
- **Text-loop** — palabras rotando (chatea/habla/investiga/vende)
- **Animated-numbers** — contadores que suben al hacer scroll
- **Text-shimmer** — brillo metálico en textos clave
- **Magnetic buttons** — CTA que siguen el cursor (desktop)
- **Cursor glow** — halo que sigue el mouse
- **Scroll-scrub** — video descompuesto frame a frame con scroll
- **Crossfade** — fusión entre dos renders 3D al hacer scroll
- **Ticker infinito** + **carrusel autoplay** + **reveal on scroll**

### Chat con voz (Web Speech API)
- Avatar animado (vtuber en canvas) que reacciona al hablar
- Micrófono → reconocimiento de voz (español)
- Respuestas con TTS (voz sintetizada)
- Sin backend de voz — todo en el navegador

### Vitrina de productos (demos reales)
| Producto | Demo |
|---|---|
| **Chatbot RAG** | Sube PDF/DOCX/TXT → embeddings → responde citando la fuente |
| **Chatbot de Noticias** | NewsAPI en vivo (español) |
| **Chatbot Investigativo** | Papers académicos reales (arXiv + OpenAlex fallback) |

## 🛠️ Arquitectura

```
aura/
├── index.html    # Todo el frontend (HTML+CSS+JS, ~50KB)
├── server.py     # Backend: HTTP/1.1 + anti-inyección + endpoints
└── videos/       # Assets de video (orbes 3D generados con IA)
```

### Backend (server.py)
- `POST /api/chat` — LLM vía gateway (anti-inyección deny-first + RAG con contexto)
- `POST /api/upload` — archivo → extracción de texto (PDF/DOCX/TXT/MD) → chunks → embeddings
- `POST /api/noticias` — NewsAPI (español)
- `POST /api/investigar` — arXiv con fallback a OpenAlex
- Servido con HTTP/1.1 + Content-Length + Range 206 (streaming de video)

### Infraestructura
- systemd + Nginx reverse proxy + HTTPS (cert Let's Encrypt)
- RAG: embeddings MiniLM (384d) + similitud coseno en memoria
- LLM: gateway gratuito (OpenAI-compatible)

## 🔒 Seguridad
- Anti-inyección deny-first (patrones multilenguaje + Unicode)
- Límites de tamaño (10MB upload, 800 chars pregunta)
- Las API keys se leen del entorno (`.env` del servidor), nunca del repo

## 🚀 Despliegue
```bash
# 1. Configurar keys en /root/.hermes/.env
#    FREELLMAPI_KEY=...  (gateway LLM)
#    NEWSAPI_KEY=...     (noticias)

# 2. systemd
cp aura.service /etc/systemd/system/
systemctl daemon-reload && systemctl enable --now aura

# 3. Nginx proxy host → 127.0.0.1:8944 + certbot
```

## 📄 Licencia
MIT — uso libre con atribución a BMOPS Consulting.

---

© 2026 BMOPS Consulting — [bmops.tech](https://bmops.tech)
