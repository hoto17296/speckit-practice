FROM python:3.14

COPY --from=ghcr.io/astral-sh/uv:0.9 /uv /usr/local/bin/uv
ENV UV_PROJECT_ENVIRONMENT=/usr/local
ENV UV_TOOL_BIN_DIR=/usr/local/bin

RUN uv tool install specify-cli --from git+https://github.com/github/spec-kit.git