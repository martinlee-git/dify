# 읽어보기

- 원문 저장소: `langgenius/dify`
- 미러 저장소: `martinlee-git/dify`
- 원문 문서: https://github.com/langgenius/dify/blob/main/packages/dev-proxy/README.md
- 미러 경로: `packages/dev-proxy/README.md`

## 한글 요약

@langgenius/dev 프록시 프론트엔드 프로젝트를 위한 일반 Hono 기반 개발 프록시입니다. 패키지는 제품별 경로, 쿠키 이름 또는 환경 변수 규칙을 제공하지 않습니다. 모든 프록시된 경로와 업스트림 대상은 로컬 구성 파일에 선언됩니다. 설치 프런트엔드 프로젝트에 스크립트를 추가합니다. 다음을 사용하여 실행합니다. CLI 지원되는 옵션: config, c: 구성 파일 경로. 기본값은 dev Proxy.config.ts입니다. env 파일: 구성 파일을 평가하기 전에 환경 변수를 로드합니다. 호스트: 구성에서 server.host를 재정의합니다. 포트: 구성에서 server.port를 재정의합니다. 감시: 구성 및 환경 파일 변경 사항을 다시 로드합니다. 기본적으로 활성화되어 있습니다. 감시 없음: 구성 및 환경 파일 다시 로드를 비활성화합니다. help, h: 도움말을 인쇄합니다. 대상이 지원되지 않습니다. 경로와 업스트림이 명시적으로 유지되도록 구성 파일에 대상을 넣습니다. CLI는 기본적으로 구성 파일과 명시적 env 파일을 감시합니다. 경로, CORS, 대상 및 쿠키 재작성 변경 사항은 실행 중인 프로세스에 적용됩니다. 확인된 호스트 또는 포트가 변경되면 프록시가 닫힙니다.

## 핵심 발췌

이전 서버를 삭제하고 새 서버를 시작합니다. 구성 모양 구성 파일은 .ts, .mts, .js 또는 .mjs일 수 있습니다. 경로는 선언 순서에 따라 일치합니다. 첫 번째로 일치하는 경로가 승리합니다. Each configured path matches both the exact path and all child paths, so paths: '/api' matches /api, /api/apps, and /api/apps/123. 기본적으로 자격 증명 CORS는 localhost, 127.0.0.1 및 ::1과 같은 로컬 개발 원본에 허용됩니다. 특정 원본으로 제한하려면: 시나리오 1: 하나의 로컬 경로 그룹을 온라인 백엔드로 프록시 로컬 프런트엔드가 하나의 프록시 서버를 통해 온라인 백엔드를 호출해야 하는 경우 이 방법을 사용합니다. 예를 들어 프런트엔드는 http://127.0.0.1:5001/api/apps를 호출하고 프록시는 이를 https://cloud.example.com/api/apps로 전달합니다. Optional .env: Command: Edits to ./.env.local reload the proxy automatically. 에스

## 원문 내용

# @langgenius/dev-proxy

Generic Hono-based development proxy for frontend projects. The package does not ship any product-specific routes, cookie names, or environment variable conventions. Every proxied path and upstream target is declared in a local config file.

## Installation

```bash
pnpm add -D @langgenius/dev-proxy
```

Add a script in your frontend project:

```json
{
  "scripts": {
    "dev:proxy": "dev-proxy --config ./dev-proxy.config.ts --env-file ./.env.local"
  }
}
```

Run it with:

```bash
pnpm dev:proxy
```

## CLI

```bash
dev-proxy --config ./dev-proxy.config.ts
```

Supported options:

- `--config`, `-c`: config file path. Defaults to `dev-proxy.config.ts`.
- `--env-file`: load environment variables before evaluating the config file.
- `--host`: override `server.host` from config.
- `--port`: override `server.port` from config.
- `--watch`: reload config and env file changes. Enabled by default.
- `--no-watch`: disable config and env file reloads.
- `--help`, `-h`: print help.

`--target` is not supported. Put targets in the config file so routes and upstreams stay explicit.

The CLI watches the config file and the explicit `--env-file` by default. Route, CORS, target, and cookie rewrite changes are applied in the running process. If the resolved host or port changes, the proxy closes the old server and starts a new one.

## Config Shape

```ts
import { defineDevProxyConfig } from '@langgenius/dev-proxy'

export default defineDevProxyConfig({
  server: {
    host: '127.0.0.1',
    port: 5001,
  },
  routes: [
    {
      paths: '/api',
      target: 'https://example.com',
    },
  ],
  cors: {
    allowedOrigins: 'local',
  },
})
```

Config files can be `.ts`, `.mts`, `.js`, or `.mjs`.

`routes` are matched in declaration order. The first matching route wins. Each configured path matches both the exact path and all child paths, so `paths: '/api'` matches `/api`, `/api/apps`, and `/api/apps/123`.

By default, credentialed CORS is allowed for local development origins such as `localhost`, `127.0.0.1`, and `::1`. To restrict it to specific origins:

```
cors: {
  allowedOrigins: ['http://localhost:3000'],
}
```

## Scenario 1: Proxy One Local Route Group To An Online Backend

Use this when a local frontend should call an online backend through one proxy server. For example, the frontend calls `http://127.0.0.1:5001/api/apps`, and the proxy forwards it to `https://cloud.example.com/api/apps`.

```ts
import { defineDevProxyConfig } from '@langgenius/dev-proxy'

const target = process.env.DEV_PROXY_TARGET || 'https://cloud.example.com'

export default defineDevProxyConfig({
  server: {
    host: process.env.DEV_PROXY_HOST || '127.0.0.1',
    port: Number(process.env.DEV_PROXY_PORT || 5001),
  },
  routes: [
    {
      paths: '/api',
      target,
    },
  ],
})
```

Optional `.env`:

```env
DEV_PROXY_TARGET=https://cloud.example.com
DEV_PROXY_HOST=127.0.0.1
DEV_PROXY_PORT=5001
```

Command:

```bash
dev-proxy --config ./dev-proxy.config.ts --env-file ./.env.local
```

Edits to `./.env.local` reload the proxy automatically.

## Scenario 2: Proxy Two Route Groups To Two Local Backends

Use this when one frontend needs to talk to two different local services. For example:

- `/console/api/*` goes to a local console backend at `http://127.0.0.1:5001`
- `/api/*` goes to a local public API backend at `http://127.0.0.1:5002`

```ts
import { defineDevProxyConfig } from '@langgenius/dev-proxy'

const consoleApiTarget = process.env.DEV_PROXY_CONSOLE_API_TARGET || 'http://127.0.0.1:5001'
const publicApiTarget = process.env.DEV_PROXY_PUBLIC_API_TARGET || 'http://127.0.0.1:5002'

export default defineDevProxyConfig({
  server: {
    host: process.env.DEV_PROXY_HOST || '127.0.0.1',
    port: Number(process.env.DEV_PROXY_PORT || 8082),
  },
  routes: [
    {
      paths: '/console/api',
      target: consoleApiTarget,
    },
    {
      paths: '/api',
      target: publicApiTarget,
    },
  ],
})
```

Optional `.env`:

```env
DEV_PROXY_CONSOLE_API_TARGET=http://127.0.0.1:5001
DEV_PROXY_PUBLIC_API_TARGET=http://127.0.0.1:5002
DEV_PROXY_HOST=127.0.0.1
DEV_PROXY_PORT=8082
```

When two route groups overlap, put the more specific one first:

```ts
routes: [
  { paths: '/api/enterprise', target: 'http://127.0.0.1:5003' },
  { paths: '/api', target: 'http://127.0.0.1:5002' },
]
```

## Cookie Rewrite

Cookie rewriting is opt-in and config-driven. The package does not know any application cookie names.

Use `cookieRewrite` when an upstream uses secure cookie prefixes such as `__Host-` or `__Secure-`, but local development needs cookies to work over `http://localhost`.

```ts
import type { CookieRewriteOptions } from '@langgenius/dev-proxy'
import { defineDevProxyConfig } from '@langgenius/dev-proxy'

const cookieRewrite: CookieRewriteOptions = {
  hostPrefixCookies: ['access_token', 'refresh_token', /^passport-/],
}

export default defineDevProxyConfig({
  routes: [
    {
      paths: '/api',
      target: 'https://cloud.example.com',
      cookieRewrite,
    },
  ],
})
```

Set `cookieRewrite: false` to disable cookie rewriting for a route.

When one local proxy can point to multiple online targets, use `localCookieScope: 'target-origin'`
for auth cookies. The proxy stores configured cookies under target-specific local names,
forwards only the active target's cookies upstream, and can override a stale frontend CSRF
header from the active scoped cookie:

```ts
const cookieRewrite: CookieRewriteOptions = {
  hostPrefixCookies: ['access_token', 'csrf_token', 'refresh_token'],
  localCookieScope: 'target-origin',
  csrfHeader: {
    cookieName: 'csrf_token',
    headerName: 'X-CSRF-Token',
  },
}
```

## Behavior

- The proxy preserves the matched path prefix when forwarding requests.
- Request bodies are forwarded as streams.
- Hop-by-hop headers are removed before forwarding.
- Local credentialed CORS and preflight requests are handled by the proxy.
- Route matching is explicit and order-sensitive.