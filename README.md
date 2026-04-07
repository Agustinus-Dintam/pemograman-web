# Fullstack Web App

Proyek ini adalah contoh kerangka aplikasi web *(Full-Stack)* yang terisolasi dengan **Docker**. Semua kode dapat dijalankan langsung menggunakan container tanpa perlu menginstal dependensi bahasa pemrograman di perangkat Anda secara lokal.

## 🚀 Teknologi yang Digunakan
* **Frontend:** Vite, React, TypeScript
* **Backend:** Golang (Gin Framework)
* **Database:** PostgreSQL
* **Orchestration:** Docker & Docker Compose
* **CI/CD Pipeline:** GitHub Actions

## 📂 Struktur Proyek
```text
.
├── frontend/             # Root folder aplikasi React + Vite
│   ├── Dockerfile.dev    # Docker image untuk development
│   └── Dockerfile.prod   # Docker image multi-stage untuk production (Nginx)
├── backend/              # Root folder aplikasi Golang
│   ├── Dockerfile.dev    # Docker image untuk development dengan auto-reload (Air)
│   └── Dockerfile.prod   # Docker image multi-stage untuk binary API yang sangat ringan
├── docker-compose.yml         # Konfigurasi Dev (default, dengan host file mapping)
├── docker-compose.staging.yml # Konfigurasi Staging override (dibangun statis tanpa mapping ke host)
└── docker-compose.prod.yml    # Konfigurasi Production (restart-policy diaktifkan, load balancer, statis)
```

## 🛠️ Panduan Menjalankan Aplikasi

Aplikasi ini menggunakan Docker Compose override pattern. Pastikan Anda sudah menginstal Docker dan Docker Compose.

### Lingkungan Development (Auto-Reloading Aktif)
Pada lingkungan develoment, setiap kali Anda mengedit kode di folder `frontend` maupun `backend`, maka aplikasi/port yang ada di container Docker akan ter-*refresh* secara otomatis.
```bash
docker compose up --build
```
* **Frontend UI:** `http://localhost:5173`
* **API Backend (Healthcheck):** `http://localhost:8080/api/ping`

### Lingkungan Production
Lingkungan production dikompilasi hingga menjadi statik (Nginx untuk frontend, minimal binary untuk Golang) guna menjamin performa terbaik.
```bash
docker compose -f docker-compose.yml -f docker-compose.prod.yml up --build -d
```
* **Frontend Web:** `http://localhost` (Port HTTP 80)
* **API Backend:** `http://localhost:8080/api/ping`

## 👨‍💻 CI/CD
Proyek ini dilengkapi dengan **[GitHub Actions](.github/workflows/ci.yml)** yang secara otomatis menguji pembangunan *image* Docker (`build`) Frontend maupun Backend untuk mendeteksi *error* kompilasi pada saat kode di-push.
