# 읽어보기

- 원문 저장소: `langgenius/dify`
- 미러 저장소: `martinlee-git/dify`
- 원문 문서: https://github.com/langgenius/dify/blob/main/docs/weaviate/WEAVIATE_MIGRATION_GUIDE/README.md
- 미러 경로: `docs/weaviate/WEAVIATE_MIGRATION_GUIDE/README.md`

## 한글 요약

Weaviate 마이그레이션 가이드: v1.19 → v1.27 개요 Dify는 Python 클라이언트가 v3.24에서 v4.17로 업데이트되면서 Weaviate v1.19에서 v1.27로 업그레이드되었습니다. 주요 변경 사항 1. Weaviate Server : 1.19.0 → 1.27.0 1. Python Client : weaviate client =3.24.0 → weaviate client==4.17.0 1. gRPC 필수 : Weaviate v1.27에는 gRPC 포트 50051 필요(HTTP 포트 8080에 추가) 1. Docker Compose : 클라이언트 설치를 위한 임시 진입점 재정의 추가 Key 개선 사항 gRPC를 통한 더 빠른 벡터 작업 향상된 일괄 처리 향상된 오류 처리 Docker 사용자를 위한 마이그레이션 단계 1단계: 데이터 백업 2단계: Dify 업데이트 3단계: 서비스 시작 4단계: 소스 설치를 위한 마이그레이션 확인 1단계: 종속성 업데이트 2단계: Weaviate 서버 업데이트 문제 해결 오류: "'weaviate.classes'라는 모듈이 없습니다." 해결책: 오류: "gRPC 상태 확인 실패" 해결책: 오류: "Weaviate 버전 1.19.0은 그렇지 않습니다. 지원됨" 솔루션: 데이터 마이그레이션 실패 솔루션: 롤백 지침 구성 요소

## 핵심 발췌

능력 | 구성요소 | 이전 버전 | 새 버전 | 호환 가능 | | | | | | | 위비에이트 서버 | 1.19.0 | 1.27.0 | ✅ 예 | | 클라이언트를 위축시키다 | 3.24.0 | ==4.17.0 | ✅ 예 | | 기존 데이터 | v1.19 형식 | v1.27 형식 | ✅ 예 | 테스트 체크리스트 프로덕션에 배포하기 전: [ ] 모든 Weaviate 데이터 백업 [ ] 스테이징 환경에서 테스트 [ ] 기존 컬렉션에 액세스할 수 있는지 확인 [ ] 벡터 검색 기능 테스트 [ ] 문서 업로드 및 검색 테스트 [ ] gRPC 연결 안정성 모니터링 [ ] 성능 메트릭 확인 지원 문제가 발생하는 경우: 1. GitHub 문제 확인: https://github.com/langgenius/dify/issues 1. 오류 메시지 Docker 로그: docker compose 로그 weaviate Dify 버전 마이그레이션 단계 시도 중요 참고 사항 데이터 안전 : 기존 벡터 dat

## 원문 내용

# Weaviate Migration Guide: v1.19 → v1.27

## Overview

Dify has upgraded from Weaviate v1.19 to v1.27 with the Python client updated from v3.24 to v4.17.

## What Changed

### Breaking Changes

1. **Weaviate Server**: `1.19.0` → `1.27.0`
1. **Python Client**: `weaviate-client~=3.24.0` → `weaviate-client==4.17.0`
1. **gRPC Required**: Weaviate v1.27 requires gRPC port `50051` (in addition to HTTP port `8080`)
1. **Docker Compose**: Added temporary entrypoint overrides for client installation

### Key Improvements

- Faster vector operations via gRPC
- Improved batch processing
- Better error handling

## Migration Steps

### For Docker Users

#### Step 1: Backup Your Data

```bash
cd docker
docker compose down
sudo cp -r ./volumes/weaviate ./volumes/weaviate_backup_$(date +%Y%m%d)
```

#### Step 2: Update Dify

```bash
git pull origin main
docker compose pull
```

#### Step 3: Start Services

```bash
docker compose up -d
sleep 30
curl http://localhost:8080/v1/meta
```

#### Step 4: Verify Migration

```bash
# Check both ports are accessible
curl http://localhost:8080/v1/meta
netstat -tulpn | grep 50051

# Test in Dify UI:
# 1. Go to Knowledge Base
# 2. Test search functionality
# 3. Upload a test document
```

### For Source Installation

#### Step 1: Update Dependencies

```bash
cd api
uv sync --dev
uv run python -c "import weaviate; print(weaviate.__version__)"
# Should show: 4.17.0
```

#### Step 2: Update Weaviate Server

```bash
cd docker
docker compose -f docker-compose.middleware.yaml --profile weaviate up -d weaviate
curl http://localhost:8080/v1/meta
netstat -tulpn | grep 50051
```

## Troubleshooting

### Error: "No module named 'weaviate.classes'"

**Solution**:

```bash
cd api
uv sync --reinstall-package weaviate-client
uv run python -c "import weaviate; print(weaviate.__version__)"
# Should show: 4.17.0
```

### Error: "gRPC health check failed"

**Solution**:

```bash
# Check Weaviate ports
docker ps | grep weaviate
# Should show: 0.0.0.0:8080->8080/tcp, 0.0.0.0:50051->50051/tcp

# If missing gRPC port, add to docker-compose:
# ports:
#   - "8080:8080"
#   - "50051:50051"
```

### Error: "Weaviate version 1.19.0 is not supported"

**Solution**:

```bash
# Update Weaviate image in docker-compose
# Change: semitechnologies/weaviate:1.19.0
# To: semitechnologies/weaviate:1.27.0
docker compose down
docker compose up -d
```

### Data Migration Failed

**Solution**:

```bash
cd docker
docker compose down
sudo rm -rf ./volumes/weaviate
sudo cp -r ./volumes/weaviate_backup_YYYYMMDD ./volumes/weaviate
docker compose up -d
```

## Rollback Instructions

```bash
# 1. Stop services
docker compose down

# 2. Restore data backup
sudo rm -rf ./volumes/weaviate
sudo cp -r ./volumes/weaviate_backup_YYYYMMDD ./volumes/weaviate

# 3. Checkout previous version
git checkout <previous-commit>

# 4. Restart services
docker compose up -d
```

## Compatibility

| Component | Old Version | New Version | Compatible |
|-----------|-------------|-------------|------------|
| Weaviate Server | 1.19.0 | 1.27.0 | ✅ Yes |
| weaviate-client | ~3.24.0 | ==4.17.0 | ✅ Yes |
| Existing Data | v1.19 format | v1.27 format | ✅ Yes |

## Testing Checklist

Before deploying to production:

- [ ] Backup all Weaviate data
- [ ] Test in staging environment
- [ ] Verify existing collections are accessible
- [ ] Test vector search functionality
- [ ] Test document upload and retrieval
- [ ] Monitor gRPC connection stability
- [ ] Check performance metrics

## Support

If you encounter issues:

1. Check GitHub Issues: https://github.com/langgenius/dify/issues
1. Create a bug report with:
   - Error messages
   - Docker logs: `docker compose logs weaviate`
   - Dify version
   - Migration steps attempted

## Important Notes

- **Data Safety**: Existing vector data remains fully compatible
- **No Re-indexing**: No need to rebuild vector indexes
- **Temporary Workaround**: The entrypoint overrides are temporary until next Dify release
- **Performance**: May see improved performance due to gRPC usage