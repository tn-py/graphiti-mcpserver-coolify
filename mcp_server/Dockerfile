FROM python:3.11-slim

# Set working directory
WORKDIR /app

# Install uv and required system utilities
RUN apt-get update && \
    apt-get install -y curl wget && \
    curl -LsSf https://astral.sh/uv/install.sh | sh && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

# Add uv to PATH
ENV PATH="/root/.local/bin:${PATH}"

# Set default host binding and output buffering
ENV MCP_SERVER_HOST=0.0.0.0 \
    PYTHONUNBUFFERED=1

# Copy project metadata and install dependencies
COPY pyproject.toml .
RUN uv sync

# Copy application source code
COPY graphiti_mcp_server.py .

# Expose MCP server port (used for SSE)
EXPOSE 8000

# Default CMD (actual arguments like --transport are passed via docker-compose.yml)
CMD ["uv", "run", "graphiti_mcp_server.py"]
