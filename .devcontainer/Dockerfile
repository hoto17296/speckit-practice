FROM node:lts

# Claude Code
RUN npm install -g @anthropic-ai/claude-code

# uv
COPY --from=ghcr.io/astral-sh/uv:0.9 /uv /usr/local/bin/uv
ENV UV_PROJECT_ENVIRONMENT=/usr/local
ENV UV_PYTHON_INSTALL_DIR=/usr/local/uv/python
ENV UV_PYTHON_BIN_DIR=/usr/local/bin
ENV UV_TOOL_BIN_DIR=/usr/local/bin

# Python
RUN uv python install 3.14 --default

# SpecKit CLI
RUN uv tool install specify-cli --from git+https://github.com/github/spec-kit.git