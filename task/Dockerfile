FROM debian:bookworm-slim

# --- Base OS packages: Python, Node.js, and common build/dev tools ---
RUN apt-get update && apt-get install -y \
    curl \
    ca-certificates \
    gnupg \
    python3 \
    python3-pip \
    python3-venv \
    build-essential \
    git \
    tmux \
    unzip \
    vim \
    jq \
    && curl -fsSL https://deb.nodesource.com/setup_20.x | bash - \
    && apt-get install -y nodejs \
    && rm -rf /var/lib/apt/lists/*

RUN node -v && npm -v && python3 --version

# --- Claude Code ---
RUN curl -fsSL https://claude.ai/install.sh | bash -s 2.1.212

# Disable Claude Code's background auto-updater globally
RUN mkdir -p /root/.claude && \
    cat <<'EOF' > /root/.claude/settings.json
{
  "env": {
    "DISABLE_AUTOUPDATER": "1"
  }
}
EOF

# --- uv (Python package/dependency manager) ---
RUN curl -LsSf https://astral.sh/uv/install.sh | sh

ENV PATH="/root/.local/bin:${PATH}"

# --- cli-trace-extractor setup ---
COPY cli-trace-extractor.zip /root/cli-trace-extractor.zip
RUN unzip -q /root/cli-trace-extractor.zip -d /root/ \
    && rm /root/cli-trace-extractor.zip

WORKDIR /root/cli-trace-extractor
RUN uv sync

RUN timeout 5 uv run mitmdump || true
RUN test -f /root/.mitmproxy/mitmproxy-ca-cert.pem

RUN cp /root/.mitmproxy/mitmproxy-ca-cert.pem /usr/local/share/ca-certificates/mitmproxy.crt \
    && update-ca-certificates

WORKDIR /workspace
CMD ["/bin/bash"]
