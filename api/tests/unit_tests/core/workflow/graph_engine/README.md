# 읽어보기

- 원문 저장소: `langgenius/dify`
- 미러 저장소: `martinlee-git/dify`
- 원문 문서: https://github.com/langgenius/dify/blob/main/api/tests/unit_tests/core/workflow/graph_engine/README.md
- 미러 경로: `api/tests/unit_tests/core/workflow/graph_engine/README.md`

## 한글 요약

작업흐름 그래프 엔진 연기 테스트 이제 이 디렉토리는 외부 그래폰 패키지 주위에 Dify 소유의 작은 연기 레이어만 유지합니다. 유지된 적용 범위는 다음 사항에 중점을 둡니다. 1. 워크플로 레이어 정의: 레이어/테스트 llm quota.py 레이어/테스트 observability.py 2. 인간 입력 재개 통합: 병렬 인간 입력 조인 이력서.py 테스트 3. 하나의 모의 도구/chatflow 스모크 경로: chatflow.py의 테스트 도구 아래 도우미 모듈은 유지된 스모크 테스트에서 사용하기 때문에 남아 있습니다. 1. 모의 config.py 테스트 2. 모의 공장 테스트 3. 모의 노드.py 테스트 4. 테스트 테이블 러너.py 예:

## 핵심 발췌

작업흐름 그래프 엔진 연기 테스트 이제 이 디렉토리는 외부 그래폰 패키지 주위에 Dify 소유의 작은 연기 레이어만 유지합니다. 유지된 적용 범위는 다음 사항에 중점을 둡니다. 1. 작업 흐름 레이어 정의: `layers/test llm quot

## 원문 내용

# Workflow Graph Engine Smoke Tests

This directory now keeps only a small Dify-owned smoke layer around the external
`graphon` package.

Retained coverage focuses on:

1. Dify workflow layers:
   - `layers/test_llm_quota.py`
   - `layers/test_observability.py`
2. Human-input resume integration:
   - `test_parallel_human_input_join_resume.py`
3. One mocked tool/chatflow smoke path:
   - `test_tool_in_chatflow.py`

The helper modules below remain only because the retained smoke tests use them:

1. `test_mock_config.py`
2. `test_mock_factory.py`
3. `test_mock_nodes.py`
4. `test_table_runner.py`

Examples:

```bash
uv run --project api --dev pytest api/tests/unit_tests/core/workflow/graph_engine/layers/test_llm_quota.py
uv run --project api --dev pytest api/tests/unit_tests/core/workflow/graph_engine/layers/test_observability.py
uv run --project api --dev pytest api/tests/unit_tests/core/workflow/graph_engine/test_parallel_human_input_join_resume.py
uv run --project api --dev pytest api/tests/unit_tests/core/workflow/graph_engine/test_tool_in_chatflow.py
```