# Add proxy setup
ENV http_proxy="http://163.116.128.80:8080"
ENV https_proxy="http://163.116.128.80:8080"

# Install curl if not already available
RUN apt-get update && apt-get install -y curl

# Install uv using Astral's official installer (it downloads a binary, not via pip)
RUN curl -Ls https://astral.sh/uv/install.sh | sh

# Add uv to PATH
ENV PATH="/root/.cargo/bin:/root/.local/bin:/app/.local/bin:/root/.uv/bin:$PATH"
