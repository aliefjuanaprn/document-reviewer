# Enterprise Collaborative PDF Review

Aplikasi review PDF realtime dengan Next.js dan FastAPI.

## Struktur

- `frontend-nextjs/` menangani UI Next.js, login, documents dashboard, workspace review, dan konsumsi API FastAPI.
- `fastapi-realtime/` menangani database, storage PDF, auth, share link, participant session, comment CRUD, annotation, LiveKit token voice, dan WebSocket realtime.
- `nginx/` berisi contoh reverse proxy untuk `/share/{token}` dan `/ws/{document_uuid}`.

## Voice

Voice menggunakan LiveKit.

```env
LIVEKIT_URL=https://voices.owa.my.id
LIVEKIT_API_KEY=APIKEY123
LIVEKIT_API_SECRET=SECRET123
```

FastAPI akan mengeluarkan JWT LiveKit untuk room `document-{uuid}` dengan identity peserta review.

## Alur Utama

1. Owner login di Next.js.
2. Owner upload PDF ke FastAPI.
3. Owner klik `Bagikan`; FastAPI membuat token `/share/{token}`.
4. Reviewer membuka link tanpa login.
5. Reviewer mengisi nama dan email.
6. FastAPI membuat participant session dengan `session_uuid`.
7. Workspace terbuka dan frontend berkomunikasi dengan FastAPI untuk annotation/comment.
8. Frontend meminta token LiveKit ke FastAPI untuk voice review.
9. FastAPI broadcast ke semua participant dalam room `document_uuid`.
