# 읽어보기

- 원문 저장소: `langgenius/dify`
- 미러 저장소: `martinlee-git/dify`
- 원문 문서: https://github.com/langgenius/dify/blob/main/cli/README.md
- 미러 경로: `cli/README.md`

## 한글 요약

[Dify] 플랫폼용 difyctl CLI 클라이언트입니다. 브라우저 장치 흐름 로그인, 앱 목록/검사, 구조화된 입력으로 실행, 출력을 JSON, YAML 또는 인간 텍스트로 구문 분석합니다. 설치(에지, 내부) 커밋당 에지 빌드가 Cloudflare R2에 게시됩니다. 설치 프로그램 스크립트는 이 저장소에 있습니다. 바이너리는 DIFYCTL R2 BASE(내부 공유)를 통해 R2에서 가져옵니다. | 환경 | 기본값 | 목적 | | | | | | DIFYCTL R2 베이스 | — (필수) | R2 공공 기반, 예: https://pub ….r2.dev. | | DIFYCTL 채널 | 가장자리 | 설치할 채널입니다. | | DIFYCTL 설치 디렉토리 | $HOME/.local/bin | 바이너리가 기록되는 디렉터리(<dir /difyctl). | | DIFYCTL 버전 | 최신 | 정확한 게시 버전을 고정하세요. | | DIFYCTL 커밋 | 최신 | git commit(짧은 또는 전체 sha)으로 고정합니다. | | DIFYCTL R2 접두사 | difyctl | 포인터 JSON(manifest.json / index.json)에 대한 R2 키 루트입니다. | | DIFYCTL R2 BIN 접두사 | difyctl/bin | 바이너리용 R2 키 루트(수명 주기/TTL 대상) | 기본적으로 채널 포인터(최신 빌드)가 설치됩니다. 세트D

## 핵심 발췌

특정 과거 빌드를 설치하기 위한 IFYCTL COMMIT(예: ce4af86) 또는 DIFYCTL VERSION — 둘 다 채널의 index.json을 통해 확인됩니다. Windows: $env:DIFYCTL R2 BASE='<BASE '; irm https://raw.githubusercontent.com/langgenius/dify/main/cli/scripts/install r2.ps1 | iex(동일한 환경 변수, 예: $env:DIFYCTL COMMIT='ce4af86'). 업그레이드하려면 다시 실행하세요. 태그가 지정된 rc/stable 빌드의 경우 GitHub 설치 프로그램(cli.sh 설치)을 사용하세요. 빠른 시작 배경 문서: difyctl 도움말 계정, difyctl 외부 도움말, difyctl 도움말 환경, difyctl 도움말 에이전트. 명령 전체 명령 목록을 보려면 difyctl help를 실행하십시오. 명령 참조별로 difyctl <cmd 도움말을 실행하세요. 에이전트(및 스크립팅)의 경우 difyctl 도움말 에이전트(교차 명령 운영 가이드(출력, 검색, 인증, 종료 코드, 오류, HITL, 재시도))로 시작하세요. 모든 도움 서핑

## 원문 내용

# difyctl

CLI client for [Dify] platform. Browser device-flow signin, list/inspect apps, run with structured input, parse output as JSON, YAML, or human text.

## Install (edge, internal)

Per-commit `edge` builds are published to Cloudflare R2. The installer script lives in this repo; binaries are fetched from R2 via `DIFYCTL_R2_BASE` (shared internally):

```sh
curl -fsSL https://raw.githubusercontent.com/langgenius/dify/main/cli/scripts/install-r2.sh | DIFYCTL_R2_BASE=<BASE> sh
```

| Env                     | Default            | Purpose                                                             |
| ----------------------- | ------------------ | ------------------------------------------------------------------- |
| `DIFYCTL_R2_BASE`       | — (required)       | R2 public base, e.g. `https://pub-….r2.dev`.                        |
| `DIFYCTL_CHANNEL`       | `edge`             | Channel to install.                                                 |
| `DIFYCTL_INSTALL_DIR`   | `$HOME/.local/bin` | Directory the binary is written to (`<dir>/difyctl`).               |
| `DIFYCTL_VERSION`       | latest             | Pin an exact published version.                                     |
| `DIFYCTL_COMMIT`        | latest             | Pin by git commit (short or full sha).                              |
| `DIFYCTL_R2_PREFIX`     | `difyctl`          | R2 key root for the pointer JSONs (`manifest.json` / `index.json`). |
| `DIFYCTL_R2_BIN_PREFIX` | `difyctl/bin`      | R2 key root for binaries (the lifecycle/TTL target).                |

By default the channel pointer (latest build) is installed. Set `DIFYCTL_COMMIT` (e.g. `ce4af86`) or `DIFYCTL_VERSION` to install a specific past build — both resolve through the channel's `index.json`:

```sh
curl -fsSL https://raw.githubusercontent.com/langgenius/dify/main/cli/scripts/install-r2.sh | DIFYCTL_R2_BASE=<BASE> DIFYCTL_COMMIT=ce4af86 sh
```

Windows: `$env:DIFYCTL_R2_BASE='<BASE>'; irm https://raw.githubusercontent.com/langgenius/dify/main/cli/scripts/install-r2.ps1 | iex` (same env vars, e.g. `$env:DIFYCTL_COMMIT='ce4af86'`).

Re-run to upgrade. For tagged `rc`/`stable` builds, use the GitHub installer (`install-cli.sh`).

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