# 읽어보기

- 원문 저장소: `langgenius/dify`
- 미러 저장소: `martinlee-git/dify`
- 원문 문서: https://github.com/langgenius/dify/blob/main/cli/README.md
- 미러 경로: `cli/README.md`

## 한글 요약

[Dify] 플랫폼용 difyctl CLI 클라이언트입니다. 브라우저 장치 흐름 로그인, 앱 목록/검사, 구조화된 입력으로 실행, 출력을 JSON, YAML 또는 인간 텍스트로 구문 분석합니다. 설치 빌드는 GitHub Actions 워크플로 아티팩트로 게시된 독립 실행형 바이너리(Bun 컴파일)입니다. npm이나 GitHub 릴리스 자산은 없습니다. 설치 프로그램은 메인에서 실행되는 성공적인 최신 cli release.yml을 가져오고 sha256을 확인한 다음 바이너리를 $HOME/.local/bin/difyctl에 복사합니다. | 환경 | 기본값 | 목적 | | | | | | GH 토큰 | — | 작업이 포함된 GitHub PAT(또는 GITHUB TOKEN):읽기. | | DIFYCTL 접두사 | $HOME/.local | 루트를 설치합니다. 바이너리는 <prefix /bin/difyctl. | | DIFYCTL 레포 | langgenius/dify | 소스 저장소. | | DIFYCTL 지점 | 메인 | 최근에 성공한 실행을 선택하는 분기입니다. | 지원 대상: darwin arm64, darwin x64, linux arm64, linux x64, windows x64.exe. 쉘 설치 프로그램은 Linux + macOS를 포함합니다. Windows 사용자는 동일한 아티팩트에서 .exe를 직접 다운로드할 수 있습니다. 빠른 시작 배경 문서: difyctl 도움말 계정,

## 핵심 발췌

difyctl 도움말 외부, difyctl 도움말 환경. 명령 전체 명령 목록을 보려면 difyctl help를 실행하세요. 명령 참조별로 difyctl <cmd 도움말을 실행하세요. 출력 형식 | 플래그 | 행동 | | | | | (없음) | 휴먼 테이블, 열 크기가 터미널에 자동으로 조정됩니다. | | 오 넓은 | 테이블과 동일하며 열 잘림이 없습니다. | | 오 JSON | 예쁘게 인쇄된 JSON, 기계 분석 가능, 안정적인 모양. | | 오 야믈 | o json의 YAML 미러입니다. | | 오 이름 | ID만, 개행으로 구분 - xargs로 파이프됩니다. | | 오 텍스트 | kubectl 설명 스타일 휴먼 텍스트(설명, 실행). | 오류는 JSON 모드에서 JSON 봉투를 stderr로 내보냅니다. 그렇지 않으면 인간의 메시지입니다. 종료 코드는 결정적입니다. 구성 | OS | 구성 경로 | | | | | 리눅스 | ${XDG 구성 홈: $HOME/.config}/difyctl/ | | macOS | $HOME/.config/difyctl/ | | 윈도우 | %APPDATA%\difyctl\ | 오버리

## 원문 내용

# difyctl

CLI client for [Dify] platform. Browser device-flow signin, list/inspect apps, run with structured input, parse output as JSON, YAML, or human text.

## Install

Builds are standalone binaries (Bun-compiled) published as **GitHub Actions workflow artifacts** — no npm, no GitHub Release assets. The installer fetches the latest successful `cli-release.yml` run on `main`, verifies sha256, and copies the binary into `$HOME/.local/bin/difyctl`.

```sh
# GH_TOKEN with `actions:read` scope is required — workflow artifact downloads
# need auth even on public repos.
export GH_TOKEN=<your-pat>
curl -fsSL https://raw.githubusercontent.com/langgenius/dify/main/cli/scripts/install-cli.sh | sh
```

| Env              | Default           | Purpose                                               |
| ---------------- | ----------------- | ----------------------------------------------------- |
| `GH_TOKEN`       | —                 | GitHub PAT (or `GITHUB_TOKEN`) with `actions:read`.   |
| `DIFYCTL_PREFIX` | `$HOME/.local`    | Install root. Binary lands at `<prefix>/bin/difyctl`. |
| `DIFYCTL_REPO`   | `langgenius/dify` | Source repo.                                          |
| `DIFYCTL_BRANCH` | `main`            | Branch to pick the latest successful run from.        |

Supported targets: `darwin-arm64`, `darwin-x64`, `linux-arm64`, `linux-x64`, `windows-x64.exe`. The shell installer covers Linux + macOS; Windows users can download the `.exe` directly from the same artifact.

## Quickstart

```sh
difyctl auth login                                       # opens browser; paste the device code shown
difyctl get app                                          # list apps in default workspace
difyctl describe app <app-id>                            # inspect parameters
difyctl run app <app-id> "hello"                         # run, blocking
difyctl run app <app-id> "hello" -o json | jq .answer    # JSON output
difyctl run app <app-id> --input name=world --input topic=cats   # workflow inputs
```

Background docs: `difyctl help account`, `difyctl help external`, `difyctl help environment`.

## Commands

Run `difyctl --help` for the full list of commands.
Run `difyctl <cmd> --help` for per-command reference.

## Output formats

| Flag      | Behavior                                               |
| --------- | ------------------------------------------------------ |
| (none)    | Human table, columns auto-sized to terminal.           |
| `-o wide` | Same as table, no column truncation.                   |
| `-o json` | Pretty-printed JSON, machine-parseable, stable shape.  |
| `-o yaml` | YAML mirror of `-o json`.                              |
| `-o name` | IDs only, newline-separated — pipes into `xargs`.      |
| `-o text` | kubectl-describe style human text (`describe`, `run`). |

Errors emit JSON envelope to stderr in `-o json` mode; else human message. Exit codes deterministic.

## Configuration

| OS      | Config path                                  |
| ------- | -------------------------------------------- |
| Linux   | `${XDG_CONFIG_HOME:-$HOME/.config}/difyctl/` |
| macOS   | `$HOME/.config/difyctl/`                     |
| Windows | `%APPDATA%\difyctl\`                         |

Override with `DIFY_CONFIG_DIR=/some/path`. Files written `0600`, directory `0700`. Tokens use OS keychain by default, fall back to sealed file on hosts without one.

For every env var `difyctl` reads, run `difyctl env list` (machine-readable) or `difyctl help environment` (narrative).

## Streaming

`run app` uses blocking transport by default. For long-running apps (likely exceed ~30s) pass `--stream`:

```sh
difyctl run app app-1 "tell me about cats" --stream
```

Agent apps (`mode === 'agent-chat'` or `is_agent` flag set) stream regardless — Dify backend rejects blocking requests for agent mode. Combining `--stream` with `-o json` or `-o yaml` aggregates SSE events into same envelope shape as blocking response, so structured output identical regardless of transport.

## HTTP retry

Idempotent requests (`GET`, `PUT`, `DELETE`) retry on transient network/DNS failures with exponential backoff. Default count: **3**. `POST` and `PATCH` never retry — side effects possible.

| Knob                     | Effect                                         |
| ------------------------ | ---------------------------------------------- |
| `--http-retry <n>`       | Per-invocation override. `0` disables retries. |
| `DIFYCTL_HTTP_RETRY=<n>` | Process-level default.                         |

Resolution: flag → env → 3.

## Contributing

See [`ARD.md`] for architecture patterns, scaffolding recipe, dev workflow.

## License

Apache-2.0.

[Dify]: https://dify.ai
[`ARD.md`]: ARD.md