# Docker container for Agent evaluation.

Dockerfile includes tmux and claude installation.

Use repo/ for creation of initial repo (it is ignored).

Place initial evaluation code from repo under task/workspace/A|B.

**Check claude version in Dockerfile before building.**

docker compose down
docker compose build --no-cache
docker compose up -d
docker compose exec devbox bash
