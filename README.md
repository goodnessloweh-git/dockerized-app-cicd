# dockerized-app-cicd

A containerized web app with a **complete CI/CD pipeline** — build, test, container scan, push to
GitHub Container Registry, and a deploy step — plus uptime monitoring guidance. The reference
implementation for the "DevOps My App" learning project.

## Stack

- **App:** small Flask service with a `/health` endpoint
- **Container:** multi-stage-friendly Dockerfile, runs as non-root
- **CI/CD:** GitHub Actions — lint → test → build → Trivy scan → push to GHCR → deploy
- **Monitoring:** UptimeRobot/Healthchecks guidance on the `/health` endpoint

## Pipeline

```
lint (ruff) → test (pytest) → docker build → Trivy scan → push GHCR → deploy
```

## Run locally

```bash
pip install -r requirements.txt
python app.py            # http://localhost:8080
# or with Docker:
docker build -t dockerized-app-cicd .
docker run -p 8080:8080 dockerized-app-cicd
```

## What this demonstrates

- A real, green CI/CD pipeline (not a toy) with tests and a security gate
- Container best practices: non-root user, healthcheck, slim base image
- Image publishing to GHCR on push to main
- Monitoring a deployed service

## Endpoints

| Path | Purpose |
|------|---------|
| `/` | App index (JSON) |
| `/health` | Liveness/uptime check (used by monitoring) |

## Monitoring

Point [UptimeRobot](https://uptimerobot.com/) or Healthchecks at `https://<your-host>/health` with a
5-minute interval and email/Slack alerts. The endpoint returns `200 {"status":"ok"}`.

## License

MIT
