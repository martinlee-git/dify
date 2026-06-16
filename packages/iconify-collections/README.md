# 읽어보기

- 원문 저장소: `langgenius/dify`
- 미러 저장소: `martinlee-git/dify`
- 원문 문서: https://github.com/langgenius/dify/blob/main/packages/iconify-collections/README.md
- 미러 경로: `packages/iconify-collections/README.md`

## 한글 요약

@dify/iconify 컬렉션 Dify 사용자 정의 SVG 아이콘을 위해 사전 생성된 Iconify 컬렉션입니다. 웹 앱은 이 패키지에서 이러한 컬렉션을 가져오므로 Tailwind는 개발 시작 중에 이전 웹/앱/구성 요소/base/icons/src 트리에서 사용자 정의 SVG 아이콘 데이터를 스캔하고 빌드할 필요가 없습니다. 사용자 정의 SVG 아이콘 추가 다음 디렉토리 중 하나에 새 SVG 소스 파일을 추가합니다. 자산/공개/... 다중 색상 또는 공개 브랜드와 같은 아이콘의 경우. Assets/vender/... currentColor로 렌더링해야 하는 UI 공급업체 아이콘용입니다. SVG 파일을 추가하거나 변경한 후 패키지된 컬렉션을 다시 생성합니다. 그런 다음 차원 가드를 실행합니다. 이렇게 하면 컬렉션 병합 후 20x20으로 유지되어야 하는 기본 탐색 아이콘과 같이 레이아웃에 민감한 고유 크기로 기존 아이콘 그룹을 보호합니다. custom public/ 또는 custom vender/에서 SVG 소스 파일과 생성된 패키지 파일을 모두 커밋합니다. 아이콘을 다시 생성한 후 웹 개발 서버를 다시 시작합니다. Tailwind는 시작 시 이 플러그인 컬렉션을 로드하므로 이미 실행 중인 개발 서버는

## 핵심 발췌

다시 시작될 때까지 새로 추가된 사용자 정의 클래스를 렌더링하지 않습니다. 프런트엔드 코드에서 Tailwind 아이콘 클래스를 통해 생성된 아이콘을 사용합니다. 예: 다음과 같습니다. 새로운 사용자 정의 SVG 아이콘에 대해 새로 생성된 React 아이콘 구성 요소 또는 JSON 파일을 web/app/comComponents/base/icons/src/... 아래에 추가하지 마세요. 그 길은 유산입니다. 새로운 사용자 정의 아이콘은 이 패키지를 통해 전달되어야 하며 사용자 정의 클래스로 사용되어야 합니다. 생성된 Icons.json 차이점을 검토할 때 관련되지 않은 기존 아이콘 그룹이 고유한 너비와 높이를 잃거나 변경하지 않았는지 확인하세요. 그룹이 레이아웃에 민감한 경우 이를 scripts/check icon Dimensions.ts에 추가하세요.

## 원문 내용

# @dify/iconify-collections

Pre-generated Iconify collections for Dify custom SVG icons. The web app imports these collections from this package so Tailwind does not need to scan and build custom SVG icon data from the old `web/app/components/base/icons/src` tree during dev startup.

## Adding Custom SVG Icons

Add new SVG source files under one of these directories:

- `assets/public/...` for multi-color or public brand-like icons.
- `assets/vender/...` for UI vendor icons that should render with `currentColor`.

After adding or changing SVG files, regenerate the packaged collections:

```bash
pnpm --filter @dify/iconify-collections generate
```

Then run the dimension guard:

```bash
pnpm --filter @dify/iconify-collections check:dimensions
```

This protects existing icon groups with layout-sensitive intrinsic sizes, such as the `main-nav-*` icons that must remain `20x20` after collection flattening.

Commit both the SVG source files and the generated package files under `custom-public/` or `custom-vender/`.
Restart the web dev server after regenerating icons. Tailwind loads this plugin collection at startup, so an already-running dev server may not render newly-added `i-custom-*` classes until it restarts.

Use the generated icons through Tailwind icon classes in frontend code. For example:

```text
assets/vender/integrations/mcp.svg
```

becomes:

```tsx
<span aria-hidden className="i-custom-vender-integrations-mcp size-4" />
```

Do not add new generated React icon components or JSON files under `web/app/components/base/icons/src/...` for new custom SVG icons. That path is legacy; new custom icons should flow through this package and be consumed as `i-custom-*` classes.

When reviewing generated `icons.json` diffs, check that unrelated existing icon groups did not lose or change their intrinsic `width` and `height`. If a group is layout-sensitive, add it to `scripts/check-icon-dimensions.ts`.