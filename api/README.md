# 읽어보기

- 원문 저장소: `langgenius/dify`
- 미러 저장소: `martinlee-git/dify`
- 원문 문서: https://github.com/langgenius/dify/blob/main/api/README.md
- 미러 경로: `api/README.md`

## 한글 요약

Dify 백엔드 API 설정 및 실행 [!IMPORTANT] v1.3.0 릴리스에서는 Dify API 백엔드 서비스의 패키지 관리자로 poem이 uv로 대체되었습니다. 아래 설정 및 개발 명령을 실행하려면 uv 및 pnpm이 필요합니다. 스크립트 사용(권장) 스크립트는 해당 위치를 기준으로 경로를 확인하므로 어디에서나 실행할 수 있습니다. 1. 설정을 실행합니다(env 파일 복사 및 종속성 설치). 1. api/.env, web/.env.local 및 docker/middleware.env 값을 검토합니다(아래 SECRET KEY 참고 사항 참조). 1. 미들웨어(PostgreSQL/Redis/Weaviate)를 시작합니다. 1. 백엔드를 시작합니다(마이그레이션을 먼저 실행합니다). 1. Diify 웹 서비스를 시작하세요. ./dev/setup 및 ./dev/start web은 저장소 루트 작업 공간을 통해 JavaScript 종속성을 설치하므로 별도의 cd web && pnpm 설치 단계가 필요하지 않습니다. 1. http://localhost:3000을 방문하여 애플리케이션을 설정합니다. 1. 작업자 서비스를 시작합니다(비동기 및 스케줄러 작업, API에서 실행). 1. 선택 사항: Celery Beat(예약된 작업)를 시작합니다. 환경 참고 사항 [!IMPORTANT]

## 핵심 발췌

프런트엔드와 백엔드가 서로 다른 하위 도메인에서 실행되는 경우 COOKIE DOMAIN을 사이트의 최상위 도메인(예: example.com)으로 설정하세요. 인증 쿠키를 공유하려면 프런트엔드와 백엔드가 동일한 최상위 도메인에 있어야 합니다. .env 파일에서 SECRET KEY를 생성합니다. Linux용 bash Mac용 bash 테스트 1. 백엔드 및 테스트 환경 모두에 대한 종속성 설치 1. pyproject.toml의 tool.pytest env 섹션에서 모의 ​​시스템 환경 변수를 사용하여 로컬로 테스트를 실행합니다. Claude.md를 더 많이 확인할 수 있습니다. TS 스텁 생성 https://jsontotable.org/openapi를 사용하여 typescript를 typescript로 변환하여 typescript로 변환합니다.

## 원문 내용

# Dify Backend API

## Setup and Run

> [!IMPORTANT]
>
> In the v1.3.0 release, `poetry` has been replaced with
> [`uv`](https://docs.astral.sh/uv/) as the package manager
> for Dify API backend service.

`uv` and `pnpm` are required to run the setup and development commands below.

### Using scripts (recommended)

The scripts resolve paths relative to their location, so you can run them from anywhere.

1. Run setup (copies env files and installs dependencies).

   ```bash
   ./dev/setup
   ```

1. Review `api/.env`, `web/.env.local`, and `docker/middleware.env` values (see the `SECRET_KEY` note below).

1. Start middleware (PostgreSQL/Redis/Weaviate).

   ```bash
   ./dev/start-docker-compose
   ```

1. Start backend (runs migrations first).

   ```bash
   ./dev/start-api
   ```

1. Start Dify [web](../web) service.

   ```bash
   ./dev/start-web
   ```

   `./dev/setup` and `./dev/start-web` install JavaScript dependencies through the repository root workspace, so you do not need a separate `cd web && pnpm install` step.

1. Set up your application by visiting `http://localhost:3000`.

1. Start the worker service (async and scheduler tasks, runs from `api`).

   ```bash
   ./dev/start-worker
   ```

1. Optional: start Celery Beat (scheduled tasks).

   ```bash
   ./dev/start-beat
   ```

### Environment notes

> [!IMPORTANT]
>
> When the frontend and backend run on different subdomains, set COOKIE_DOMAIN to the site’s top-level domain (e.g., `example.com`). The frontend and backend must be under the same top-level domain in order to share authentication cookies.

- Generate a `SECRET_KEY` in the `.env` file.

  bash for Linux

  ```bash
  sed -i "/^SECRET_KEY=/c\\SECRET_KEY=$(openssl rand -base64 42)" .env
  ```

  bash for Mac

  ```bash
  secret_key=$(openssl rand -base64 42)
  sed -i '' "/^SECRET_KEY=/c\\
  SECRET_KEY=${secret_key}" .env
  ```

## Testing

1. Install dependencies for both the backend and the test environment

   ```bash
   cd api
   uv sync --group dev
   ```

1. Run the tests locally with mocked system environment variables in `tool.pytest_env` section in `pyproject.toml`, more can check [Claude.md](../CLAUDE.md)

   ```bash
   cd api
   uv run pytest                           # Run all tests
   uv run pytest tests/unit_tests/         # Unit tests only
   uv run pytest tests/integration_tests/  # Integration tests

   # Code quality
   ./dev/reformat               # Run all formatters and linters
   uv run ruff check --fix ./   # Fix linting issues
   uv run ruff format ./        # Format code
   uv run pyrefly check         # Type checking
   ```

## Generate TS stub

```
uv run dev/generate_swagger_specs.py --output-dir openapi
```

use https://jsontotable.org/openapi-to-typescript to convert to typescript