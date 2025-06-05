# Install curl, required build tools
RUN apt-get update && apt-get install -y curl build-essential pkg-config libssl-dev

# Install uv using Astral's install script
RUN curl -Ls https://astral.sh/uv/install.sh | sh

# Add uv to PATH (where install.sh places it)
ENV PATH="/root/.cargo/bin:$PATH"

# (Optional) Verify uv installation
RUN uv --version
