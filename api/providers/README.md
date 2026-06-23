# 읽어보기

- 원문 저장소: `langgenius/dify`
- 미러 저장소: `martinlee-git/dify`
- 원문 문서: https://github.com/langgenius/dify/blob/main/api/providers/README.md
- 미러 경로: `api/providers/README.md`

## 한글 요약

공급자 이 디렉토리에는 Dify의 API 코어에 연결되는 선택적 작업 공간 패키지가 들어 있습니다. 공급자는 인터페이스를 구현하고 API 코어에 등록하는 일을 담당합니다. 공급자 메커니즘을 사용하면 선택한 공급자 집합으로 소프트웨어를 구축하여 배포의 보안과 유연성을 향상시킬 수 있습니다. 공급자 개발 VDB 공급자 테스트 공급자 테스트는 종종 패키지 옆에 있습니다. 공급자/<유형 /<백엔드 /tests/단위 테스트/. 공유 설비는 공급자(예: conftest.py) 아래에 있을 수 있습니다. 공급자 제외 선택한 공급자를 사용하여 구축하려면 no group vdb all 및 no group Trace all을 사용하여 기본 공급자를 비활성화한 다음 group vdb <provider 및 그룹 추적 <provider를 사용하여 특정 공급자를 활성화합니다.

## 핵심 발췌

공급자 이 디렉토리에는 Dify의 API 코어에 연결되는 선택적 작업 공간 패키지가 들어 있습니다. 공급자는 인터페이스를 구현하고 API 코어에 등록하는 일을 담당합니다. 공급자 메커니즘

## 원문 내용

# Providers

This directory holds **optional workspace packages** that plug into Dify’s API core. Providers are responsible for implementing the interfaces and registering themselves to the API core. Provider mechanism allows building the software with selected set of providers so as to enhance the security and flexibility of distributions.

## Developing Providers

- [VDB Providers](vdb/README.md)

## Tests

Provider tests often live next to the package, e.g. `providers/<type>/<backend>/tests/unit_tests/`. Shared fixtures may live under `providers/` (e.g. `conftest.py`).

## Excluding Providers

In order to build with selected providers, use `--no-group vdb-all` and `--no-group trace-all` to disable default ones, then use `--group vdb-<provider>` and `--group trace-<provider>` to enable specific providers.