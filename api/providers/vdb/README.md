# 읽어보기

- 원문 저장소: `langgenius/dify`
- 미러 저장소: `martinlee-git/dify`
- 원문 문서: https://github.com/langgenius/dify/blob/main/api/providers/vdb/README.md
- 미러 경로: `api/providers/vdb/README.md`

## 한글 요약

VDB 공급자 이 디렉터리에는 모든 VDB 공급자가 포함되어 있습니다. 아키텍처 1. Core(api/core/rag/datasource/vdb/)는 계약을 정의하고 플러그인을 로드합니다. 2. 각 공급자(api/providers/vdb/<backend /)는 해당 계약을 구현하고 진입점을 등록합니다. 3. 런타임 시 importlib.metadata.entry points는 백엔드 이름(예: pgVector)을 팩토리 클래스로 확인합니다. 레지스트리는 로드된 클래스를 캐시합니다(벡터 백엔드 Registry.py 참조). 인터페이스 | 조각 | 역할 | | | | | AbstractVectorFactory | 당신은 이것을 하위 분류합니다. 초기화 벡터(데이터 세트, 속성, 임베딩) BaseVector를 구현합니다. 선택적으로 새 데이터 세트에 대해 gen index struct dict()를 사용하십시오. | | 베이스벡터 | 귀하의 상점 클래스는 생성, 텍스트 추가, 벡터 검색, 삭제 등을 하위 클래스로 분류합니다. | | 벡터 유형 | 지원되는 백엔드 문자열 ID의 StrEnum입니다. 기존 백엔드처럼 선택 가능해야 하는 새 백엔드를 도입할 때 멤버를 추가하세요. | | 발견 | dify.Vector 백엔드 진입점을 로드하고 벡터 팩토리 클래스(벡터 유형)를 캐시합니다. | 그만큼

## 핵심 발췌

상위 수준 호출자는 벡터 Factory.py의 Vector입니다. 구성된 벡터 유형 또는 데이터세트별 벡터 유형을 읽고, 벡터 팩토리 클래스를 가져오고, 팩토리를 인스턴스화하고, 반환된 BaseVector 구현을 사용합니다. 진입점 이름은 벡터 유형 문자열과 일치해야 합니다. 진입점은 dify.Vector backends 그룹에 등록됩니다. 진입점 이름(왼쪽)은 정확히 다른 모든 곳에서 벡터 유형으로 사용되는 문자열이어야 합니다. 일반적으로 VectorType 열거형 값(예: PGVECTOR = "pgVector" → 진입점 이름 pgVector; TIDB ON QDRANT = "tidb on qdrant" → tidb on qdrant)입니다. pyproject.toml에서: 값은 module:attribute입니다. 가져올 수 있는 모듈 경로와 AbstractVectorFactory를 구현하는 클래스입니다. 등록 작동 방식 1. 처음 사용 시 get 벡터 팩토리 클래스(벡터 유형)가 벡터 t를 찾습니다.

## 원문 내용

# VDB providers

This directory contains all VDB providers.

## Architecture
1. **Core** (`api/core/rag/datasource/vdb/`) defines the contracts and loads plugins.
2. **Each provider** (`api/providers/vdb/<backend>/`) implements those contracts and registers an entry point.
3. At runtime, **`importlib.metadata.entry_points`** resolves the backend name (e.g. `pgvector`) to a factory class. The registry caches loaded classes (see `vector_backend_registry.py`).

### Interfaces

| Piece | Role |
|--------|----------|
| `AbstractVectorFactory`  | You subclass this. Implement `init_vector(dataset, attributes, embeddings) -> BaseVector`. Optionally use `gen_index_struct_dict()` for new datasets. |
| `BaseVector` | Your store class subclasses this: `create`, `add_texts`, `search_by_vector`, `delete`, etc. |
| `VectorType` | `StrEnum` of supported backend **string ids**. Add a member when you introduce a new backend that should be selectable like existing ones. |
| Discovery | Loads `dify.vector_backends` entry points and caches `get_vector_factory_class(vector_type)`. |

The high-level caller is `Vector` in `vector_factory.py`: it reads the configured or dataset-specific vector type, calls `get_vector_factory_class`, instantiates the factory, and uses the returned `BaseVector` implementation.

### Entry point name must match the vector type string

Entry points are registered under the group **`dify.vector_backends`**. The **entry point name** (left-hand side) must be exactly the string used as `vector_type` everywhere else—typically the **`VectorType` enum value** (e.g. `PGVECTOR = "pgvector"` → entry point name `pgvector`; `TIDB_ON_QDRANT = "tidb_on_qdrant"` → `tidb_on_qdrant`).

In `pyproject.toml`:

```toml
[project.entry-points."dify.vector_backends"]
pgvector = "dify_vdb_pgvector.pgvector:PGVectorFactory"
```

The value is **`module:attribute`**: a importable module path and the class implementing `AbstractVectorFactory`.

### How registration works

1. On first use, `get_vector_factory_class(vector_type)` looks up `vector_type` in a process cache.
2. If missing, it scans **`entry_points().select(group="dify.vector_backends")`** for an entry whose **`name` equals `vector_type`**.
3. It loads that entry (`ep.load()`), which must return the **factory class** (not an instance).
4. There is an optional internal map `_BUILTIN_VECTOR_FACTORY_TARGETS` for non-distribution builtins; **normal VDB plugins use entry points only**.

After you change a provider’s `pyproject.toml` (entry points or dependencies), run **`uv sync`** in `api/` so the installed environment’s dist-info matches the project metadata.

### Package layout (VDB)

Each backend usually follows:

- `api/providers/vdb/<backend>/pyproject.toml` — project name `dify-vdb-<backend>`, dependencies, entry points.
- `api/providers/vdb/<backend>/src/dify_vdb_<python_package>/` — implementation (e.g. `PGVector`, `PGVectorFactory`).

See `vdb/pgvector/` as a reference implementation.

### Wiring a new backend into the API workspace

The API uses a **uv workspace** (`api/pyproject.toml`):

1. **`[tool.uv.workspace]`** — `members = ["providers/vdb/*"]` already includes every subdirectory under `vdb/`; new folders there are workspace members.
2. **`[tool.uv.sources]`** — add a line for your package: `dify-vdb-mine = { workspace = true }`.
3. **`[project.optional-dependencies]`** — add a group such as `vdb-mine = ["dify-vdb-mine"]`, and list `dify-vdb-mine` under `vdb-all` if it should install with the default bundle.