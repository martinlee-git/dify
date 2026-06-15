# 읽어보기

- 원문 저장소: `langgenius/dify`
- 미러 저장소: `martinlee-git/dify`
- 원문 문서: https://github.com/langgenius/dify/blob/main/cli/test/e2e/README.md
- 미러 경로: `cli/test/e2e/README.md`

## 한글 요약

Dify CLI — E2E 테스트 스위트 라이브 Dify 서버에 대해 실제 difyctl 바이너리를 실행하는 엔드투엔드 테스트입니다. 모든 테스트는 격리된 임시 구성 디렉터리를 사용하므로 테스트 파일 간에 상태가 누출되지 않습니다. 디렉터리 레이아웃 버전 지원 difyctl은 두 가지 Dify 버전을 지원합니다. 테스트 스위트는 자동으로 조정됩니다. | 판 | DIFY E2E 에디션 | 작업공간 | EE 전용 사례 | | | | | | | 커뮤니티 에디션(CE) | ce(기본값) | 1 | 건너뛰었습니다 | | 엔터프라이즈 에디션(EE) | 어이 | 2 (auto created) | 활성 | EE 전용 테스트 케이스 Enterprise Edition 기능(독립 작업 공간 간 작업 공간 전환, 작업 공간 간 앱 쿼리 등)이 필요한 테스트는 이름에 [EE] 태그가 지정되고 helpers/skip.ts의 enterpriseOnlyIt() / enterpriseOnlyDescribe()로 래핑됩니다. CE 모드에서는 이러한 테스트가 자동으로 건너뜁니다. 설정 자격 증명 템플릿을 복사하고 값을 입력합니다. Community Edition(CE) - 최소 3개 변수 | 변수 | 설명 | | | | | DIFY E2E 호스트 | 서버 기본 URL(http://localhost) |

## 핵심 발췌

| DIFY E2E 이메일 | 계정 이메일 - 전역 설정에 의해 자동으로 생성됨 | | DIFY E2E 비밀번호 | 계정 비밀번호 | 전역 설정은 다음과 같습니다. 1. 계정 등록(멱등성 - 재실행해도 안전함) 1. 로그인 및 장치 흐름을 통해 전달자 토큰 생성 1. ​​모든 DSL 픽스처를 단일 작업 공간으로 가져오기 1. 앱 게시 및 액세스 모드 설정 → 공용 Enterprise Edition(EE) — 5개의 필수 변수 | 변수 | 설명 | | | | | DIFY E2E 에디션 | 꼭 ee | | DIFY E2E 호스트 | 콘솔/API 기본 URL | | DIFY E2E 이메일 | 회원 계정 이메일 - 엔터프라이즈 API를 통해 생성됨 | | DIFY E2E 비밀번호 | 회원 계정 비밀번호 | | DIFY E2E 엔터프라이즈 API URL | 기업 관리 API 기본 URL(https://.../inner/api) | | DIFY E2E 엔터프라이즈 API 비밀 키 | 기업 관리 API 비밀 키 | 선택사항: | 변수 | 설명

## 원문 내용

# Dify CLI — E2E Test Suite

End-to-end tests that exercise the **real `difyctl` binary** against a live
Dify server. Every test uses an isolated temporary config directory so no
state leaks between test files.

## Directory layout

```
test/e2e/
├── setup/
│   ├── env.ts              — Load & validate DIFY_E2E_* env vars (CE + EE)
│   ├── global-setup.ts     — CE/EE-aware bootstrap: account creation, token
│   │                         minting, workspace provisioning, DSL import
│   └── global-teardown.ts  — Delete conversations created during the run
│
├── helpers/
│   ├── cli.ts              — run(), withAuthFixture(), mintFreshToken(),
│   │                         injectAuth(), spawn_background()
│   ├── assert.ts           — assertExitCode, assertJson, assertErrorEnvelope,
│   │                         assertNoAnsi, assertPipeFriendlyJson, ...
│   ├── cleanup-registry.ts — registerConversation() / cleanupRegisteredConversations()
│   ├── retry.ts            — withRetry(fn, { attempts, delayMs })
│   └── skip.ts             — optionalIt(), optionalDescribe(),
│                             enterpriseOnlyIt(), enterpriseOnlyDescribe(), isEE()
│
└── suites/
    ├── auth/
    │   ├── status.e2e.ts   — auth status (text + JSON + SSO)
    │   ├── use.e2e.ts      — workspace switching ([EE] cases require 2 workspaces)
    │   ├── whoami.e2e.ts   — whoami + external SSO session checks
    │   ├── devices.e2e.ts  — devices list + revoke (runs near-last)
    │   └── logout.e2e.ts   — logout + local credential cleanup (runs last)
    ├── config/
    │   └── config.e2e.ts   — config path/get/set/unset/view, env override
    ├── discovery/
    │   ├── get-app-list.e2e.ts           — basic get app list
    │   ├── get-app-single.e2e.ts         — get single app by ID
    │   ├── describe-app.e2e.ts           — describe app
    │   └── get-app-all-workspaces.e2e.ts — get app -A ([EE] multi-workspace cases)
    └── run/
        ├── run-app-basic.e2e.ts     — basic run, -o json, --inputs, streaming,
        │                              conversation, CI mode
        ├── run-app-streaming.e2e.ts — Ctrl+C / error-event / chunk timing
        ├── run-app-file.e2e.ts      — --file upload (local + remote URL)
        └── run-app-hitl.e2e.ts      — HITL pause + resume
```

## Edition support

`difyctl` supports two Dify editions. The test suite adapts automatically:

| Edition                 | `DIFY_E2E_EDITION` | Workspaces       | EE-only cases |
| ----------------------- | ------------------ | ---------------- | ------------- |
| Community Edition (CE)  | `ce` (default)     | 1                | Skipped       |
| Enterprise Edition (EE) | `ee`               | 2 (auto-created) | Active        |

### EE-only test cases

Tests that require Enterprise Edition features (workspace switching between
independent workspaces, cross-workspace app query, etc.) are tagged `[EE]`
in their names and wrapped with `enterpriseOnlyIt()` / `enterpriseOnlyDescribe()`
from `helpers/skip.ts`. In CE mode these tests are automatically skipped.

```ts
// helpers/skip.ts usage
const eeIt = enterpriseOnlyIt(caps)
eeIt('[EE][P0] cross-workspace query returns apps from all workspaces', async () => {
  // test body
})
```

## Setup

Copy the credential template and fill in your values:

```bash
cp cli/test/e2e/.env.e2e.example cli/.env.e2e
# edit cli/.env.e2e with real credentials
```

### Community Edition (CE) — minimum 3 vars

| Variable            | Description                                           |
| ------------------- | ----------------------------------------------------- |
| `DIFY_E2E_HOST`     | Server base URL (`http://localhost`)                  |
| `DIFY_E2E_EMAIL`    | Account email — created automatically by global-setup |
| `DIFY_E2E_PASSWORD` | Account password                                      |

global-setup will:

1. Register the account (idempotent — safe to rerun)
1. Login and mint a bearer token via the device flow
1. Import all DSL fixtures into the single workspace
1. Publish apps and set access_mode → public

### Enterprise Edition (EE) — 5 required vars

| Variable                             | Description                                             |
| ------------------------------------ | ------------------------------------------------------- |
| `DIFY_E2E_EDITION`                   | Must be `ee`                                            |
| `DIFY_E2E_HOST`                      | Console/API base URL                                    |
| `DIFY_E2E_EMAIL`                     | Member account email — created via enterprise API       |
| `DIFY_E2E_PASSWORD`                  | Member account password                                 |
| `DIFY_E2E_ENTERPRISE_API_URL`        | Enterprise admin API base URL (`https://.../inner/api`) |
| `DIFY_E2E_ENTERPRISE_API_SECRET_KEY` | Enterprise admin API secret key                         |

Optional:

| Variable               | Description                                   |
| ---------------------- | --------------------------------------------- |
| `DIFY_E2E_CONSOLE_URL` | Console URL if different from `DIFY_E2E_HOST` |

global-setup will:

1. Create the member account via the enterprise admin API (idempotent)
1. Login and obtain a session cookie
1. Create two workspaces (`e2e-primary-auto`, `e2e-secondary-auto`) via the enterprise API
1. Import DSL fixtures into both workspaces
1. Publish apps and set access_mode → public via the enterprise API

### Optional overrides (both editions)

| Variable                             | Description                                      |
| ------------------------------------ | ------------------------------------------------ |
| `DIFY_E2E_TOKEN`                     | Pre-minted bearer token — skips device-flow mint |
| `DIFY_E2E_SSO_TOKEN`                 | External SSO bearer token (`dfoe_...`)           |
| `DIFY_E2E_WORKSPACE_ID`              | Override primary workspace ID                    |
| `DIFY_E2E_WORKSPACE_NAME`            | Override primary workspace name                  |
| `DIFY_E2E_WS2_ID`                    | Override secondary workspace ID (EE)             |
| `DIFY_E2E_CHAT_APP_ID`               | Override echo-chat app ID                        |
| `DIFY_E2E_WORKFLOW_APP_ID`           | Override echo-workflow app ID                    |
| `DIFY_E2E_FILE_APP_ID`               | Override file-upload app ID                      |
| `DIFY_E2E_FILE_CHAT_APP_ID`          | Override file-chat app ID                        |
| `DIFY_E2E_HITL_APP_ID`               | Override HITL main app ID                        |
| `DIFY_E2E_HITL_EXTERNAL_APP_ID`      |                                                  |
| `DIFY_E2E_HITL_SINGLE_ACTION_APP_ID` |                                                  |
| `DIFY_E2E_HITL_MULTI_NODE_APP_ID`    |                                                  |
| `DIFY_E2E_WS2_APP_ID`                | Override secondary workspace app ID (EE)         |

## Running tests

```bash
cd cli

# Community Edition (default)
bun run test:e2e

# Enterprise Edition
DIFY_E2E_EDITION=ee bun run test:e2e

# Run only [P0] smoke cases
bun run test:e2e:smoke

# Run only EE-tagged cases (P0 smoke)
DIFY_E2E_EDITION=ee bun run test:e2e:smoke --testNamePattern "\[EE\]"

# Run offline-safe config tests only (no network required)
bun run test:e2e:local

# Run a single file
bun vitest --config vitest.e2e.config.ts test/e2e/suites/auth/status.e2e.ts
```

## Test execution order

Files run sequentially (`fileParallelism: false`) in this order:

```
login → status → use → whoami → help → config → output → error-handling
  → framework → discovery → run (basic / streaming / file / HITL)
  → devices → logout
```

`devices` and `logout` run last because they revoke real server sessions.

## Design decisions

| Decision                                | Rationale                                                                                                                  |
| --------------------------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| **CE/EE edition flag**                  | `DIFY_E2E_EDITION=ce/ee` controls global-setup bootstrap path and activates/skips `[EE]`-tagged tests.                     |
| **`[EE]` tag convention**               | Test names include `[EE]` to make skipped cases visible in the report and to allow `--testNamePattern "\[EE\]"` filtering. |
| **`enterpriseOnlyIt(caps)`**            | Returns `it` in EE mode, `it.skip` in CE mode — no runtime assertions needed, skip is declarative.                         |
| **No mocking**                          | All HTTP traffic goes to the real server — this catches real integration regressions.                                      |
| **Isolated config dirs**                | Each test creates a fresh `withTempConfig()` dir; session state never leaks between tests.                                 |
| **`withAuthFixture()`**                 | Combines `withTempConfig` + `injectAuth` into a single fixture; reduces beforeEach boilerplate.                            |
| **`injectAuth()` bypasses Device Flow** | Non-auth tests skip the browser step; only `auth/` suites exercise the real flow.                                          |
| **`mintFreshToken()`**                  | `logout` and `devices-revoke` tests mint a disposable `dfoa_` token via the device flow API.                               |
| **Global `retry: 0`**                   | Flaky network calls use `withRetry()` locally; global retry masks non-idempotent failures.                                 |
| **Conversation cleanup**                | `registerConversation()` + global-teardown delete staging conversations after the run.                                     |