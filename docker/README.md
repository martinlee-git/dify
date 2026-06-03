# 읽어보기

- 원문 저장소: `langgenius/dify`
- 미러 저장소: `martinlee-git/dify`
- 원문 문서: https://github.com/langgenius/dify/blob/main/docker/README.md
- 미러 경로: `docker/README.md`

## 한글 요약

Docker 배포를 위한 README Docker Compose를 사용하여 Dify를 배포하기 위한 새로운 docker 디렉터리에 오신 것을 환영합니다. 이 README에는 기존 사용자를 위한 업데이트, 배포 지침 및 마이그레이션 세부 정보가 요약되어 있습니다. 업데이트된 내용 Certbot 컨테이너: 이제 docker compose.yaml에는 SSL 인증서 관리를 위한 certbot이 포함되어 있습니다. 이 컨테이너는 자동으로 인증서를 갱신하고 보안 HTTPS 연결을 보장합니다.\ 자세한 내용은 docker/certbot/README.md를 참조하세요. 영구 환경 변수: 필수 시작 기본값은 .env.example에 제공되고 로컬 값은 .env에 저장되어 배포 전반에 걸쳐 구성이 유지됩니다. .env란 무엇입니까? </br </br .env 파일은 로컬 시작 파일입니다. 기본 배포를 위해 .env.example에서 복사합니다. 선택적 고급 설정은 envs/ .env.example 파일에 있습니다. 통합 벡터 데이터베이스 서비스: 이제 모든 벡터 데이터베이스 서비스가 단일 Docker Compose 파일 docker compose.yaml에서 관리됩니다. 당신은 다른 ve 사이에서 전환할 수 있습니다

## 핵심 발췌

.env 파일에서 VECTOR STORE 환경 변수를 설정하여 ctor 데이터베이스를 관리합니다. docker compose.yaml을 사용하여 Dify를 배포하는 방법 1. 전제 조건: Docker 및 Docker Compose가 시스템에 설치되어 있는지 확인합니다. 2. 환경설정 : docker 디렉터리로 이동합니다. .env.example을 .env로 복사합니다. 필수 시작 기본값을 변경해야 하는 경우 .env를 사용자 정의하세요. 고급 설정이 필요한 경우 .example 접미사 없이 envs/에서 선택적 파일을 복사하세요. 선택 사항(고급 배포의 경우): .env.example에서 복사된 전체 .env 파일을 유지 관리하는 경우 환경 동기화 도구를 사용하여 사용자 지정 설정을 유지하면서 최신 .env.example 업데이트와 일치하도록 유지할 수 있습니다. 아래의 환경 변수 동기화 섹션을 참조하세요. 3. 서비스 실행 : docker compo 실행

## 원문 내용

## README for docker Deployment

Welcome to the new `docker` directory for deploying Dify using Docker Compose. This README outlines the updates, deployment instructions, and migration details for existing users.

### What's Updated

- **Certbot Container**: `docker-compose.yaml` now contains `certbot` for managing SSL certificates. This container automatically renews certificates and ensures secure HTTPS connections.\
  For more information, refer to `docker/certbot/README.md`.

- **Persistent Environment Variables**: Essential startup defaults are provided in `.env.example`, while local values are stored in `.env`, ensuring that your configurations persist across deployments.

  > What is `.env`? </br> </br>
  > The `.env` file is the local startup file. Copy it from `.env.example` for a default deployment. Optional advanced settings live in `envs/*.env.example` files.

- **Unified Vector Database Services**: All vector database services are now managed from a single Docker Compose file `docker-compose.yaml`. You can switch between different vector databases by setting the `VECTOR_STORE` environment variable in your `.env` file.

### How to Deploy Dify with `docker-compose.yaml`

1. **Prerequisites**: Ensure Docker and Docker Compose are installed on your system.
2. **Environment Setup**:
   - Navigate to the `docker` directory.
   - Copy `.env.example` to `.env`.
   - Customize `.env` when you need to change essential startup defaults. Copy optional files from `envs/` without the `.example` suffix when you need advanced settings.
   - **Optional (for advanced deployments)**:
     If you maintain a full `.env` file copied from `.env.example`, you may use the environment synchronization tool to keep it aligned with the latest `.env.example` updates while preserving your custom settings.
     See the [Environment Variables Synchronization](#environment-variables-synchronization) section below.
3. **Running the Services**:
   - Execute `docker compose up -d` from the `docker` directory to start the services.
   - To specify a vector database, set the `VECTOR_STORE` variable in your `.env` file to your desired vector database service, such as `milvus`, `weaviate`, or `opensearch`. See `envs/vectorstores/` for the full list of supported options.
   ```bash
   cp .env.example .env
   docker compose up -d
   ```

4. **SSL Certificate Setup**:
   - Refer to `docker/certbot/README.md` to set up SSL certificates using Certbot.
5. **OpenTelemetry Collector Setup**:
   - Copy `envs/core-services/shared.env.example` to `envs/core-services/shared.env`.
   - Set `ENABLE_OTEL=true` and configure `OTLP_BASE_ENDPOINT`. Tune the other `OTEL_*` knobs in the same file if needed.

### How to Deploy Middleware for Developing Dify

1. **Middleware Setup**:
   - Use the `docker-compose.middleware.yaml` for setting up essential middleware services like databases and caches.
   - Navigate to the `docker` directory.
   - Ensure the `middleware.env` file is created by running `cp envs/middleware.env.example middleware.env` (refer to the `envs/middleware.env.example` file).
2. **Running Middleware Services**:
   - Navigate to the `docker` directory.
   - Execute `docker compose --env-file middleware.env -f docker-compose.middleware.yaml -p dify up -d` to start PostgreSQL/MySQL (per `DB_TYPE`) plus the bundled Weaviate instance.

> Compose automatically loads `COMPOSE_PROFILES=${DB_TYPE:-postgresql},weaviate` from `middleware.env`, so no extra `--profile` flags are needed. Adjust variables in `middleware.env` if you want a different combination of services.

### Migration for Existing Users

For users migrating from the `docker-legacy` setup:

1. **Review Changes**: Familiarize yourself with the new `.env` configuration and Docker Compose setup.
2. **Transfer Customizations**:
   - If you have customized configurations such as `docker-compose.yaml`, `ssrf_proxy/squid.conf`, or `nginx/conf.d/default.conf`, you will need to reflect these changes in the `.env` file you create.
3. **Data Migration**:
   - Ensure that data from services like databases and caches is backed up and migrated appropriately to the new structure if necessary.

### Overview of `.env`, `.env.example`, and `envs/`

- `.env.example` contains the essential default configuration for Docker Compose deployments.
- `.env` contains local startup values copied from `.env.example` and any local changes.
- `envs/*.env.example` files contain optional advanced configuration grouped by theme.

Keep the root `.env.example` limited to variables required to start the default Docker Compose deployment.
Do not add optional, advanced, provider-specific, or service-specific variables there; place them in the appropriate `envs/*.env.example` file instead.

Docker Compose reads `envs/*.env` files when present, then reads `.env` last so values in `.env` take precedence.

#### Key Modules and Customization

- **Vector Database Services**: Depending on the type of vector database used (`VECTOR_STORE`), users can set specific endpoints, ports, and authentication details.
- **Storage Services**: Depending on the storage type (`STORAGE_TYPE`), users can configure specific settings for S3, Azure Blob, Google Storage, etc.
- **API and Web Services**: Users can define URLs and other settings that affect how the API and web frontend operate.

#### Other notable variables

The root `.env.example` file contains the essential startup settings. Optional and provider-specific settings are grouped in `envs/*.env.example` files. Here are some of the key sections and variables:

1. **Common Variables**:

   - `CONSOLE_API_URL`, `CONSOLE_WEB_URL`, `SERVICE_API_URL`, `APP_API_URL`, `APP_WEB_URL`: public URLs for the API and frontend services.
   - `SERVER_CONSOLE_API_URL`: internal URL used by the web service for server-side console API requests. Keep the default `http://api:5001` for standard Docker Compose deployments, and only change it when your web service must reach the API through a different internal address.
   - `FILES_URL`, `INTERNAL_FILES_URL`: Public and internal base URLs for file downloads and previews.
   - `ENDPOINT_URL_TEMPLATE`, `NEXT_PUBLIC_SOCKET_URL`, `TRIGGER_URL`: Additional service URLs. 
   
   See `.env.example` for the full list.

2. **Server Configuration**:

   - `LOG_LEVEL`, `DEBUG`, `FLASK_DEBUG`: Logging and debug settings.
   - `SECRET_KEY`: A key for signing sessions, JWTs, and file URLs. Leave it empty to let Dify generate a persistent key in the storage directory, or set a unique value yourself.

3. **Database Configuration**:

   - `DB_USERNAME`, `DB_PASSWORD`, `DB_HOST`, `DB_PORT`, `DB_DATABASE`: PostgreSQL database credentials and connection details.

4. **Redis Configuration**:

   - `REDIS_HOST`, `REDIS_PORT`, `REDIS_PASSWORD`: Redis server connection settings.
   - `REDIS_KEY_PREFIX`: Optional global namespace prefix for Redis keys, topics, streams, and Celery Redis transport artifacts.

5. **Celery Configuration**:

   - `CELERY_BROKER_URL`: Configuration for Celery message broker.

6. **Storage Configuration**:

   - `STORAGE_TYPE`, `OPENDAL_SCHEME`, `OPENDAL_FS_ROOT`: Default local file storage settings. Optional storage backends are configured from the files under `envs/`.

7. **Vector Database Configuration**:

   - `VECTOR_STORE`: Type of vector database (e.g., `weaviate`, `milvus`). See `envs/vectorstores/` for the full list of supported options.
   - Specific settings for each vector store like `WEAVIATE_ENDPOINT`, `MILVUS_URI`.

8. **CORS Configuration**:

   - `WEB_API_CORS_ALLOW_ORIGINS`, `CONSOLE_CORS_ALLOW_ORIGINS`: Settings for cross-origin resource sharing.

9. **OpenTelemetry Configuration**:

   - `ENABLE_OTEL`: Enable OpenTelemetry collector in api.
   - `OTLP_BASE_ENDPOINT`: Endpoint for your OTLP exporter.

10. **Other Service-Specific Environment Variables**:

    - Each service like `nginx`, `redis`, `db`, and vector databases have specific environment variables that are directly referenced in the `docker-compose.yaml`.

### Environment Variables Synchronization

When upgrading Dify or pulling the latest changes, new environment variables may be introduced in `.env.example` only when they are required for startup,
or in the optional files under `envs/` for advanced, provider-specific, and service-specific settings.

If you use the default workflow, review `.env.example` and keep your `.env` aligned with essential startup values.

If you maintain a customized `.env` file copied from `.env.example`, an optional environment variables synchronization tool is provided.

> This tool performs a **one-way synchronization** from `.env.example` to `.env`.
> Existing values in `.env` are never overwritten automatically.

#### `dify-env-sync.sh` (Optional)

This script compares your current `.env` file with the latest `.env.example` template and helps safely apply new or updated environment variables.

**What it does**

- Creates a backup of the current `.env` file before making any changes
- Synchronizes newly added environment variables from `.env.example`
- Preserves all existing custom values in `.env`
- Displays differences and variables removed from `.env.example` for review

**Backup behavior**

Before synchronization, the current `.env` file is saved to the `env-backup/` directory with a timestamped filename
(e.g. `env-backup/.env.backup_20231218_143022`).

**When to use**

- After upgrading Dify to a newer version with a full `.env` file
- When `.env.example` has been updated with new environment variables
- When managing a large or heavily customized `.env` file copied from `.env.example`

**Usage**

```bash
# Grant execution permission (first time only)
chmod +x dify-env-sync.sh

# Run the synchronization
./dify-env-sync.sh
```

### Additional Information

- **Continuous Improvement Phase**: We are actively seeking feedback from the community to refine and enhance the deployment process. As more users adopt this new method, we will continue to make improvements based on your experiences and suggestions.
- **Support**: For detailed configuration options and environment variable settings, refer to the `.env.example` file and the Docker Compose configuration files in the `docker` directory.

This README aims to guide you through the deployment process using the new Docker Compose setup. For any issues or further assistance, please refer to the official documentation or contact support.