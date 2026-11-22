# fastapi-docker-ip-service

A tiny FastAPI service, packaged with Docker, that returns the server's public IP address.

## Features

- `GET /` returns `{"Server": "Running"}`.
- `GET /ip` fetches the URL set in the `URL` environment variable (the compose file uses `https://myexternalip.com/raw`) and returns its text as `{"ip": "..."}`.
- Dockerfile and docker-compose file to run it.

## Tech stack

Python 3.10, FastAPI, Uvicorn, Requests, python-dotenv, Docker.

## Structure

```
app.py              FastAPI routes
scraper.py          Scraper class: HTTP GET of the URL from the environment
Dockerfile
docker-compose.yml  Sets URL and publishes port 8000
requirements.txt
```

## Setup and run

Environment variable:

- `URL`: address that returns the caller's public IP as plain text. Required for `/ip`. Can be put in a `.env` file.

With Docker:

```sh
docker compose up --build
```

Without Docker:

```sh
python3 -m venv venv && source venv/bin/activate
pip install -r requirements.txt
URL=https://myexternalip.com/raw python app.py
```

## Usage

```sh
curl http://localhost:8000/ip
# {"ip":"203.0.113.7"}
```

## Limitations

- Minimal demo. No error handling: if `URL` is unset or unreachable, `/ip` returns a 500.
- Despite the class name, `Scraper` only performs one GET request.

## License

MIT, see `LICENSE`.
