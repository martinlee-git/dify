# 읽어보기

- 원문 저장소: `langgenius/dify`
- 미러 저장소: `martinlee-git/dify`
- 원문 문서: https://github.com/langgenius/dify/blob/main/web/README.md
- 미러 경로: `web/README.md`

## 한글 요약

Dify Frontend 이것은 [Next.js] 프로젝트이지만 [vinext]로 개발할 수 있습니다. 시작하기 소스코드로 실행하기 웹 프론트엔드 서비스를 시작하기 전, 다음과 같은 환경이 준비되어 있는지 확인하세요. [Node.js] [pnpm] 해당 vp 명령과 함께 [Vite+]를 사용할 수도 있습니다. 예를 들어 pnpm install 대신 vp install을 사용하고 pnpm run test 대신 vp test를 사용합니다. 팁 패키지 관리자 버전을 자동으로 관리하려면 Corepack을 설치하고 활성화하는 것이 좋습니다. 자세히 알아보기: [Corepack] 리포지토리 루트에서 다음 명령을 실행합니다. 먼저 종속성을 설치합니다. [!NOTE] JavaScript 종속성은 리포지토리 루트의 작업 영역 파일(package.json, pnpm lock.yaml, pnpm Workspace.yaml 및 .nvmrc)에 의해 관리됩니다. 저장소 루트에서 종속성을 설치한 다음 web/에서 프런트엔드 스크립트를 실행합니다. 그런 다음 환경 변수를 구성합니다. web/.env.local을 생성하고 web/.env.example의 내용을 복사합니다. 귀하에 따라 이러한 환경 변수의 값을 수정하십시오.

## 핵심 발췌

r 요구 사항: [!IMPORTANT] 1. 프런트엔드와 백엔드가 서로 다른 하위 도메인에서 실행되는 경우 NEXT PUBLIC COOKIE DOMAIN=1을 설정합니다. 인증 쿠키를 공유하려면 프런트엔드와 백엔드가 동일한 최상위 도메인에 있어야 합니다. 1. NEXT PUBLIC API PREFIX 및 NEXT PUBLIC PUBLIC API PREFIX를 올바른 백엔드 API URL로 설정해야 합니다. 마지막으로 개발 서버를 실행합니다. 브라우저에서 <http://localhost:3000>을 열어 결과를 확인합니다. 웹/앱에서 파일 편집을 시작할 수 있습니다. 파일을 편집하면 페이지가 자동으로 업데이트됩니다. Deploy on server 먼저 프로덕션용 앱 빌드: 그런 다음 서버 시작: Docker 이미지를 수동으로 빌드하는 경우 저장소 루트를 빌드 컨텍스트로 사용: 호스트 및 포트를 사용자 정의하려는 경우: Storybook 이 프로젝트에서는 UI에 [Storybook]을 사용합니다.

## 원문 내용

# Dify Frontend

This is a [Next.js] project, but you can dev with [vinext].

## Getting Started

### Run by source code

Before starting the web frontend service, please make sure the following environment is ready.

- [Node.js]
- [pnpm]

You can also use [Vite+] with the corresponding `vp` commands.
For example, use `vp install` instead of `pnpm install` and `vp test` instead of `pnpm run test`.

> [!TIP]
> It is recommended to install and enable Corepack to manage package manager versions automatically:
>
> ```bash
> npm install -g corepack
> corepack enable
> ```
>
> Learn more: [Corepack]

Run the following commands from the repository root.

First, install the dependencies:

```bash
pnpm install
```

> [!NOTE]
> JavaScript dependencies are managed by the workspace files at the repository root: `package.json`, `pnpm-lock.yaml`, `pnpm-workspace.yaml`, and `.nvmrc`.
> Install dependencies from the repository root, then run frontend scripts from `web/`.

Then, configure the environment variables.
Create `web/.env.local` and copy the contents from `web/.env.example`.
Modify the values of these environment variables according to your requirements:

```bash
cp web/.env.example web/.env.local
```

> [!IMPORTANT]
>
> 1. When the frontend and backend run on different subdomains, set NEXT_PUBLIC_COOKIE_DOMAIN=1. The frontend and backend must be under the same top-level domain in order to share authentication cookies.
> 1. It's necessary to set NEXT_PUBLIC_API_PREFIX and NEXT_PUBLIC_PUBLIC_API_PREFIX to the correct backend API URL.

Finally, run the development server:

```bash
pnpm -C web run dev
# or if you are using vinext which provides a better development experience
pnpm -C web run dev:vinext
# (optional) start the dev proxy server so that you can use online API in development
# edit web/dev-proxy.config.ts to choose proxy paths
# edit web/.env.local to override DEV_PROXY_TARGET, DEV_PROXY_ENTERPRISE_TARGET, DEV_PROXY_HOST, or DEV_PROXY_PORT
pnpm -C web run dev:proxy
```

Open <http://localhost:3000> with your browser to see the result.

You can start editing the files under `web/app`.
The page auto-updates as you edit the file.

## Deploy

### Deploy on server

First, build the app for production:

```bash
pnpm -C web run build
```

Then, start the server:

```bash
pnpm -C web run start
```

If you build the Docker image manually, use the repository root as the build context:

```bash
docker build -f web/Dockerfile -t dify-web .
```

If you want to customize the host and port:

```bash
pnpm -C web run start --port=3001 --host=0.0.0.0
```

## Storybook

This project uses [Storybook] for UI component development.

To start the storybook server, run:

```bash
pnpm -C web storybook
```

Open <http://localhost:6006> with your browser to see the result.

## Lint Code

If your IDE is VSCode, rename `.vscode/settings.example.json` to `.vscode/settings.json` for lint code setting.

Then follow the [Lint Documentation] to lint the code.

## Test

We use [Vitest] and [React Testing Library] for Unit Testing.

**📖 Complete Testing Guide**: See [web/docs/test.md] for detailed testing specifications, best practices, and examples.

> [!IMPORTANT]
> As we are using Vite+, the `vitest` command is not available.
> Please make sure to run tests with `vp` commands.
> For example, use `npx vp test` instead of `npx vitest`.

Run test:

```bash
pnpm -C web test
```

> [!NOTE]
> Our test is not fully stable yet, and we are actively working on improving it.
> If you encounter test failures only in CI but not locally, please feel free to ignore them and report the issue to us.
> You can try to re-run the test in CI, and it may pass successfully.

### Example Code

If you are not familiar with writing tests, refer to:

- [index.spec.tsx] - Component test example

### Analyze Component Complexity

Before writing tests, use the script to analyze component complexity:

```bash
pnpm analyze-component app/components/your-component/index.tsx
```

This will help you determine the testing strategy. See [web/testing/testing.md] for details.

## Documentation

Visit <https://docs.dify.ai> to view the full documentation.

## Community

The Dify community can be found on [Discord community], where you can ask questions, voice ideas, and share your projects.

[Corepack]: https://github.com/nodejs/corepack#readme
[Discord community]: https://discord.gg/5AEfbxcd9k
[Lint Documentation]: ./docs/lint.md
[Next.js]: https://nextjs.org
[Node.js]: https://nodejs.org
[React Testing Library]: https://testing-library.com/docs/react-testing-library/intro
[Storybook]: https://storybook.js.org
[Vite+]: https://viteplus.dev
[Vitest]: https://vitest.dev
[index.spec.tsx]: ./app/components/base/action-button/__tests__/index.spec.tsx
[pnpm]: https://pnpm.io
[vinext]: https://github.com/cloudflare/vinext
[web/docs/test.md]: ./docs/test.md