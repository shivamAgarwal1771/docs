# Install curl if not already installed
RUN apt-get update && apt-get install -y curl

# Install uv using Astral's official install script
RUN curl -Ls https://astral.sh/uv/install.sh | sh

# Add uv to PATH
ENV PATH="/root/.cargo/bin:$PATH"
