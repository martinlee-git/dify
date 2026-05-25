# 읽어보기

- 원문 저장소: `langgenius/dify`
- 미러 저장소: `martinlee-git/dify`
- 원문 문서: https://github.com/langgenius/dify/blob/main/api/providers/vdb/vdb-clickzetta/tests/README.md
- 미러 경로: `api/providers/vdb/vdb-clickzetta/tests/README.md`

## 한글 요약

Clickzetta 통합 테스트 테스트 실행 Clickzetta 통합 테스트를 실행하려면 다음 환경 변수를 설정해야 합니다. 그런 다음 테스트를 실행합니다. 보안 참고 자격 증명을 저장소에 커밋하지 마십시오. 항상 환경 변수나 보안 자격 증명 관리 시스템을 사용하세요.

## 핵심 발췌

Clickzetta 통합 테스트 ## 테스트 실행 Clickzetta 통합 테스트를 실행하려면 다음 환경 변수를 설정해야 합니다. ```bash 내보내기 CLICKZETTA USERNAME=사용자 이름 내보내기 CLICKZETTA PASSWORD

## 원문 내용

# Clickzetta Integration Tests

## Running Tests

To run the Clickzetta integration tests, you need to set the following environment variables:

```bash
export CLICKZETTA_USERNAME=your_username
export CLICKZETTA_PASSWORD=your_password
export CLICKZETTA_INSTANCE=your_instance
export CLICKZETTA_SERVICE=api.clickzetta.com
export CLICKZETTA_WORKSPACE=your_workspace
export CLICKZETTA_VCLUSTER=your_vcluster
export CLICKZETTA_SCHEMA=dify
```

Then run the tests:

```bash
pytest api/tests/integration_tests/vdb/clickzetta/
```

## Security Note

Never commit credentials to the repository. Always use environment variables or secure credential management systems.