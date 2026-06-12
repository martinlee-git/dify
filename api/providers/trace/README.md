# 읽어보기

- 원문 저장소: `langgenius/dify`
- 미러 저장소: `martinlee-git/dify`
- 원문 문서: https://github.com/langgenius/dify/blob/main/api/providers/trace/README.md
- 미러 경로: `api/providers/trace/README.md`

## 한글 요약

추적 공급자 이 디렉터리에는 Dify 작업 추적 데이터(워크플로, 메시지, 도구, 중재 등)를 외부 관찰 백엔드(Langfuse, LangSmith, OpenTelemetry 스타일 내보내기 등)로 보내는 선택적 작업 공간 패키지가 들어 있습니다. VDB 공급자와 달리 추적 플러그인은 진입점을 통해 검색되지 않습니다. API 코어는 공급자 ID 및 매핑을 등록한 후 core/ops/ops 추적 관리자.py에서 명시적으로 패키지를 가져옵니다. 건축 | 레이어 | 위치 | 역할 | | | | | | 계약 | api/core/ops/base 추적 인스턴스.py, api/core/ops/entities/trace 엔터티.py, api/core/ops/entities/config 엔터티.py | BaseTraceInstance, BaseTracingConfig 및 형식화된 TraceInfo 페이로드 | | 레지스트리 | api/core/ops/ops 추적 관리자.py | TracingProviderEnum, OpsTraceProviderConfigMap — 공급자 문자열 → 구성 클래스, 암호화된 키 및 추적 클래스 매핑 | | 귀하의 패키지 | api/providers/trace/trace <이름 / | Pydantic 구성 + BaseTraceInstance의 하위 클래스 | 런타임 시 OpsTraceManager dec

## 핵심 발췌

저장된 자격 증명을 추출하고, 구성 모델을 구축하고, 추적 인스턴스를 캐시하고, 구체적인 BaseTraceInfo 하위 유형을 사용하여 추적(추적 정보)을 호출합니다. 구현하는 것 1. 구성 모델(BaseTracingConfig) core.ops.entities.config 엔터티에서 BaseTracingConfig 하위 클래스입니다. Pydantic 검증자를 사용하십시오. 적절한 경우 core.ops.utils의 도우미(예: URL 유효성 검사, 경로가 있는 URL 유효성 검사, 프로젝트 이름 유효성 검사)를 재사용합니다. 필드는 관리자가 사용하는 두 가지 그룹, 즉 비밀 키로 분류됩니다. 저장 시 암호화되는 필드 이름(API 키, 토큰, 비밀번호). 기타 키 — 비밀이 아닌 연결 설정(호스트, 프로젝트 이름, 엔드포인트) OpsTraceProviderConfigMap 항목에 이러한 키 이름을 나열하면 암호화/해독 및 병합 논리가 올바르게 유지됩니다. 2. 추적 인스턴스(BaseTraceInstance) 하위 클래스 BaseTraceIn

## 원문 내용

# Trace providers

This directory holds **optional workspace packages** that send Dify **ops tracing** data (workflows, messages, tools, moderation, etc.) to an external observability backend (Langfuse, LangSmith, OpenTelemetry-style exporters, and others).

Unlike VDB providers, trace plugins are **not** discovered via entry points. The API core imports your package **explicitly** from `core/ops/ops_trace_manager.py` after you register the provider id and mapping.

## Architecture

| Layer | Location | Role |
|--------|----------|------|
| Contracts | `api/core/ops/base_trace_instance.py`, `api/core/ops/entities/trace_entity.py`, `api/core/ops/entities/config_entity.py` | `BaseTraceInstance`, `BaseTracingConfig`, and typed `*TraceInfo` payloads |
| Registry | `api/core/ops/ops_trace_manager.py` | `TracingProviderEnum`, `OpsTraceProviderConfigMap` — maps provider **string** → config class, encrypted keys, and trace class |
| Your package | `api/providers/trace/trace-<name>/` | Pydantic config + subclass of `BaseTraceInstance` |

At runtime, `OpsTraceManager` decrypts stored credentials, builds your config model, caches a trace instance, and calls `trace(trace_info)` with a concrete `BaseTraceInfo` subtype.

## What you implement

### 1. Config model (`BaseTracingConfig`)

Subclass `BaseTracingConfig` from `core.ops.entities.config_entity`. Use Pydantic validators; reuse helpers from `core.ops.utils` (for example `validate_url`, `validate_url_with_path`, `validate_project_name`) where appropriate.

Fields fall into two groups used by the manager:

- **`secret_keys`** — names of fields that are **encrypted at rest** (API keys, tokens, passwords).
- **`other_keys`** — non-secret connection settings (hosts, project names, endpoints).

List these key names in your `OpsTraceProviderConfigMap` entry so encrypt/decrypt and merge logic stay correct.

### 2. Trace instance (`BaseTraceInstance`)

Subclass `BaseTraceInstance` and implement:

```python
def trace(self, trace_info: BaseTraceInfo) -> None:
    ...
```

Dispatch on the concrete type with `isinstance` (see `trace_langfuse` or `trace_langsmith` for full patterns). Payload types are defined in `core/ops/entities/trace_entity.py`, including:

- `WorkflowTraceInfo`, `WorkflowNodeTraceInfo`, `DraftNodeExecutionTrace`
- `MessageTraceInfo`, `ToolTraceInfo`, `ModerationTraceInfo`, `SuggestedQuestionTraceInfo`
- `DatasetRetrievalTraceInfo`, `GenerateNameTraceInfo`, `PromptGenerationTraceInfo`

You may ignore categories your backend does not support; existing providers often no-op unhandled types.

Optional: use `get_service_account_with_tenant(app_id)` from the base class when you need tenant-scoped account context.

### 3. Register in the API core

Upstream changes are required so Dify knows your provider exists:

1. **`TracingProviderEnum`** (`api/core/ops/entities/config_entity.py`) — add a new member whose **value** is the stable string stored in app tracing config (e.g. `"mybackend"`).
2. **`OpsTraceProviderConfigMap.__getitem__`** (`api/core/ops/ops_trace_manager.py`) — add a `match` case for that enum member returning:
   - `config_class`: your Pydantic config type
   - `secret_keys` / `other_keys`: lists of field names as above
   - `trace_instance`: your `BaseTraceInstance` subclass  
   Lazy-import your package inside the case so missing optional installs raise a clear `ImportError`.

If the `match` case is missing, the provider string will not resolve and tracing will be disabled for that app.

## Package layout

Each provider is a normal uv workspace member, for example:

- `api/providers/trace/trace-<name>/pyproject.toml` — project name `dify-trace-<name>`, dependencies on vendor SDKs
- `api/providers/trace/trace-<name>/src/dify_trace_<name>/` — `config.py`, `<name>_trace.py`, optional `entities/`, and an empty **`py.typed`** file (PEP 561) so the API type checker treats the package as typed; list `py.typed` under `[tool.setuptools.package-data]` for that import name in `pyproject.toml`.

Reference implementations: `trace-langfuse/`, `trace-langsmith/`, `trace-opik/`.

## Wiring into the `api` workspace

In `api/pyproject.toml`:

1. **`[tool.uv.sources]`** — `dify-trace-<name> = { workspace = true }`
2. **`[dependency-groups]`** — add `trace-<name> = ["dify-trace-<name>"]` and include `dify-trace-<name>` in `trace-all` if it should ship with the default bundle

After changing metadata, run **`uv sync`** from `api/`.