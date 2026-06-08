# 읽어보기

- 원문 저장소: `langgenius/dify`
- 미러 저장소: `martinlee-git/dify`
- 원문 문서: https://github.com/langgenius/dify/blob/main/cli/README.md
- 미러 경로: `cli/README.md`

## 한글 요약

[Dify] 플랫폼용 difyctl CLI 클라이언트입니다. 브라우저 장치 흐름 로그인, 앱 목록/검사, 구조화된 입력으로 실행, 출력을 JSON, YAML 또는 인간 텍스트로 구문 분석합니다. 설치 빌드는 GitHub Actions 워크플로 아티팩트로 게시된 독립 실행형 바이너리(Bun 컴파일)입니다. npm이나 GitHub 릴리스 자산은 없습니다. 설치 프로그램은 메인에서 실행되는 성공적인 최신 cli release.yml을 가져오고 sha256을 확인한 다음 바이너리를 $HOME/.local/bin/difyctl에 복사합니다. | 환경 | 기본값 | 목적 | | | | | | GH 토큰 | — | 작업이 포함된 GitHub PAT(또는 GITHUB TOKEN):읽기. | | DIFYCTL 접두사 | $HOME/.local | 루트를 설치합니다. 바이너리는 <prefix /bin/difyctl. | | DIFYCTL 레포 | langgenius/dify | 소스 저장소. | | DIFYCTL 지점 | 메인 | 최근에 성공한 실행을 선택하는 분기입니다. | 지원 대상: darwin arm64, darwin x64, linux arm64, linux x64, windows x64.exe. 쉘 설치 프로그램은 Linux + macOS를 포함합니다. Windows 사용자는 동일한 아티팩트에서 .exe를 직접 다운로드할 수 있습니다. 빠른 시작 배경 문서: difyctl 도움말 계정,

## 핵심 발췌

difyctl 도움말 외부, difyctl 도움말 환경, difyctl 도움말 에이전트. 명령 전체 명령 목록을 보려면 difyctl help를 실행하십시오. 명령 참조별로 difyctl <cmd 도움말을 실행하세요. 에이전트(및 스크립팅)의 경우 difyctl 도움말 에이전트(교차 명령 운영 가이드(출력, 검색, 인증, 종료 코드, 오류, HITL, 재시도))로 시작하세요. 모든 도움말 화면도 기계에서 읽을 수 있습니다. difyctl help o json은 전체 명령 트리와 전역 계약(종료 코드, 출력 형식, 오류 봉투, HITL 프로토콜)을 덤프하고 difyctl <cmd help o json은 하나의 명령 설명자를 반환합니다. 에이전트 스킬 difyctl Skill install은 단일 순수 위임 SKILL.md를 로컬 에이전트에 설치하여 자동으로 로드합니다. 이 기술은 명령 세트를 고정하지 않습니다. 라이브 서핑을 위해 difyctl help o json의 에이전트를 가리킵니다.

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

Background docs: `difyctl help account`, `difyctl help external`, `difyctl help environment`, `difyctl help agent`.

## Commands

Run `difyctl --help` for the full list of commands.
Run `difyctl <cmd> --help` for per-command reference.

For agents (and scripting), start with `difyctl help agent` — the cross-command operating guide (output, discovery, auth, exit codes, errors, HITL, retry). Every help surface is also machine-readable: `difyctl help -o json` dumps the whole command tree plus the global contract (exit codes, output formats, error envelope, HITL protocol), and `difyctl <cmd> --help -o json` returns one command's descriptor.

## Agent skill

`difyctl skills install` installs a single, pure-delegation `SKILL.md` into your local agents so they auto-load it. The skill does not freeze the command set — it points the agent at `difyctl help -o json` for the live surface, so it never drifts from your binary. It is embedded in the binary (version-stamped) rather than checked in.

- `difyctl skills install` — dry-run: detect installed agents (Claude Code, Codex, opencode, Cursor, pi) and print where the skill would land. Writes nothing.
- `difyctl skills install --yes` — write to every detected agent, printing each path. `--agent claude-code[,cursor]` restricts to a subset; `<dir>` forces one explicit directory (handy when your agent isn't detected).
- `difyctl skills install --stdout` — print the `SKILL.md` to stdout (for piping or self-install); writes nothing.

Detection is by config-directory existence (`~/.claude`, `~/.codex`, `~/.config/opencode`, `~/.cursor`, `~/.pi`). If a copy ever looks stale, run `difyctl version` and re-run `difyctl skills install`.

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