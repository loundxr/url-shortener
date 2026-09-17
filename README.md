# URL Shortener API

A lightweight, high-performance RESTful URL shortening service written in **Go (Golang)** with **SQLite** storage, structured logging, and end-to-end testing.

> **Credits & Acknowledgements:**  
> This project was developed as an educational project following the YouTube tutorial series by **Nikolay Tuzov** ([Николай Тузов - Golang](https://www.youtube.com/@NikolayTuzov)).

---

## Tech Stack

- **Language:** Go 1.26+
- **HTTP Router:** [go-chi/chi v5](https://github.com/go-chi/chi)
- **Database:** SQLite 3 ([mattn/go-sqlite3](https://github.com/mattn/go-sqlite3))
- **Configuration:** [cleanenv](https://github.com/ilyakaznacheev/cleanenv) (YAML + Environment variables)
- **Logging:** Standard library `log/slog` (with custom `slogpretty` and `slogdiscard` handlers)
- **RPC / Integration:** gRPC client for SSO authentication integration
- **Testing:** Unit tests, table-driven tests, mock-based testing ([vektra/mockery](https://github.com/vektra/mockery), [testify](https://github.com/stretchr/testify)), and functional E2E tests

---

## Features

- **Save URL:** Generate a unique short alias for any long URL (or supply a custom alias).
- **Redirect:** Instant HTTP `302 Found` redirection from a short alias to the target URL.
- **Delete URL:** Remove shortened URL aliases.
- **Random String Generator:** Cryptographically secure pseudo-random alias generation.
- **SSO Integration:** gRPC client for interacting with a separate Single Sign-On (SSO) service.
- **Comprehensive Testing:** Handlers tested with mocks; functional E2E tests in `tests/`.

---

## API Endpoints

| Method   | Endpoint   | Description                           |
| :------- | :--------- | :------------------------------------ |
| `POST`   | `/url`     | Save a URL and generate a short alias |
| `GET`    | `/{alias}` | Redirect to the original URL          |
| `DELETE` | `/{alias}` | Delete a shortened URL alias          |

### Example Request (`POST /url`):
```json
{
  "url": "https://github.com/loundxr/url-shortener",
  "alias": "my-repo"
}
```

### Example Response:
```json
{
  "status": "OK",
  "alias": "my-repo"
}
```

---

## Project Structure

```text
.
├── cmd/
│   └── url-shortener/       # Application entry point (main.go)
├── config/                  # Configuration files (local.yaml)
├── internal/
│   ├── clients/sso/grpc/    # gRPC client for external SSO service
│   ├── config/              # Configuration loading logic
│   ├── http-server/         # HTTP handlers (save, redirect, delete) & middlewares
│   ├── lib/                 # Reusable helpers (slog handlers, random generator, response helpers)
│   └── storage/sqlite/      # SQLite database layer & migrations
├── storage/                 # Local SQLite database file (storage.db)
└── tests/                   # End-to-end (E2E) functional tests
```

---

## Getting Started

### Prerequisites
- [Go 1.26+](https://go.dev/) installed.

### Installation & Run

1. **Clone the repository:**
   ```bash
   git clone https://github.com/loundxr/url-shortener.git
   cd url-shortener
   ```

2. **Download dependencies:**
   ```bash
   go mod download
   ```

3. **Run the service:**
   ```bash
   go run cmd/url-shortener/main.go --config=./config/local.yaml
   ```

4. **Run tests:**
   ```bash
   # Run all unit tests
   go test ./...

   # Run functional / E2E tests
   go test -v ./tests/...
   ```
---