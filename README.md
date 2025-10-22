# Second-Brain

My second-brain project.

## Setup

### Docker Compose Setup (Recommended)

This project uses Docker Compose to run Open WebUI with LiteLLM as a unified AI gateway.

#### Prerequisites
- Docker and Docker Compose installed
- API keys for your preferred AI providers (OpenAI, Anthropic, etc.)

#### Configuration

1. Copy the example environment file:
   ```bash
   cp .env.example .env
   ```

2. Generate secure keys:
   ```bash
   # Generate master key for LiteLLM
   openssl rand -hex 32

   # Generate database password
   openssl rand -hex 16
   ```

3. Edit `.env` and add your keys:
   ```bash
   # Use the generated keys
   LITELLM_MASTER_KEY=<generated-master-key>

   # Update database password in both places
   DATABASE_URL=postgresql://llmproxy:<generated-db-password>@postgres:5432/litellm
   POSTGRES_PASSWORD=<generated-db-password>

   # Add your API provider keys
   OPENAI_API_KEY=your_actual_openai_key
   ANTHROPIC_API_KEY=your_actual_anthropic_key
   ```

4. (Optional) Customize `litellm-config.yaml` to add/remove models

#### Running the Stack

Start all services:
```bash
docker-compose up -d
```

Stop all services:
```bash
docker-compose down
```

View logs:
```bash
docker-compose logs -f
```

#### Services

- **Open WebUI**: http://localhost:3000 - Chat interface
- **LiteLLM**: http://localhost:4000 - AI gateway/proxy
- **PostgreSQL**: Internal database for LiteLLM

#### Authentication

By default, Open WebUI requires login. To disable authentication, uncomment the following line in `docker-compose.yml`:
```yaml
# - WEBUI_AUTH=False
```

## Notes

### Manual Docker Run (Alternative)

#### Getting the Image

Slim Image Variant

For environments with limited storage or bandwidth, Open WebUI offers slim image variants that exclude pre-bundled models. These images are significantly smaller but download required models (whisper, embedding models) on first use.

```bash
docker pull ghcr.io/open-webui/open-webui:main-slim
```

#### Running the Image

- With Login
```bash
docker run -d -p 3000:8080 -v open-webui:/app/backend/data --name open-webui ghcr.io/open-webui/open-webui:main-slim
```

- Without Login
```bash
docker run -d -p 3000:8080 -e WEBUI_AUTH=False -v open-webui:/app/backend/data --name open-webui ghcr.io/open-webui/open-webui:main-slim
```
