# 읽어보기

- 원문 저장소: `langgenius/dify`
- 미러 저장소: `martinlee-git/dify`
- 원문 문서: https://github.com/langgenius/dify/blob/main/packages/dify-ui/README.md
- 미러 경로: `packages/dify-ui/README.md`

## 한글 요약

@langgenius/dify ui 공유 UI 기본 요소, 디자인 토큰, CSS 우선 Tailwind 스타일 및 Dify의 웹/앱에서 사용하는 cn() 유틸리티입니다. 프리미티브는 cva + cn 및 Dify 디자인 토큰으로 스타일이 지정된 [기본 UI] 헤드리스 구성 요소를 둘러싼 얇고 독선적인 래퍼입니다. 업스트림 구성 요소 문서의 경우 [기본 UI 문서 색인]에서 시작하세요. private: true — 이 패키지는 pnpm 작업 공간을 통해 web/에서 사용되며 npm에 게시되지 않습니다. API를 Dify 내부로 취급하지만 작업 공간 내에서는 안정적입니다. 설치 이미 web/package.json에 작업 공간 종속성으로 연결되어 있습니다. 설치할 것이 없습니다. 새 작업 공간 소비자의 경우 다음을 추가합니다. 가져오기 항상 하위 경로에서 가져오기 내보내기 - 배럴 없음: @langgenius/dify ui에서 가져오기(하위 경로 없음)는 의도적으로 지원되지 않습니다. 이는 트리 흔들기를 사소한 것으로 유지하고 기본 요소별로 Storybook/테스트 적용 범위 속성을 만듭니다. 프리미티브 | 카테고리 | 하위 경로 | 메모 | | | | | | 작업 | ./버튼 | cva 변형을 사용한 디자인 시스템 CTA 기본 요소입니다. | | 통제 수단

## 핵심 발췌

| ./세그먼트 제어 | 모드, 필터 및 보기 선택을 위한 SegmentedControl입니다. | | 디스플레이 | ./축소 가능, ./kbd | 축소 가능한 공개 기본 요소; 키보드 입력 및 단축키 키캡 기본 요소. | | 피드백 | ./미터, ./토스트 | 측정기는 인라인 상태입니다. Toast는 z 60 레이어를 소유하고 있습니다. | | 양식 | ./form, ./field, ./fieldset, ./input, ./textarea, ./checkbox, ./checkbox 그룹, ./radio, ./radio 그룹, ./number 필드, ./select, ./slider, ./switch | 기본 양식 경계, 필드 의미 체계 및 컨트롤. | | 레이아웃 | ./스크롤 영역 | 호스트 뷰포트 위의 사용자 정의 스타일 스크롤 막대입니다. | | 미디어 | ./아바타 | 아바타 루트, 이미지 및 대체 기본 요소. | | 탐색 | ./파일 트리, ./페이지 매김, ./tabs | 미리보기 지향 파일 공개 목록을 위한 FileTree; 페이지 탐색을 위한 페이지 매김; 패널용 탭. | |

## 원문 내용

# @langgenius/dify-ui

Shared UI primitives, design tokens, CSS-first Tailwind styles, and the `cn()` utility consumed by Dify's `web/` app.

The primitives are thin, opinionated wrappers around [Base UI] headless components, styled with `cva` + `cn` and Dify design tokens.
For upstream component docs, start from the [Base UI docs index].

> `private: true` — this package is consumed by `web/` via the pnpm workspace and is not published to npm. Treat the API as internal to Dify, but stable within the workspace.

## Installation

Already wired as a workspace dependency in `web/package.json`. Nothing to install.

For a new workspace consumer, add:

```jsonc
{
  "dependencies": {
    "@langgenius/dify-ui": "workspace:*"
  }
}
```

## Imports

Always import from a **subpath export** — there is no barrel:

```ts
import { Button } from '@langgenius/dify-ui/button'
import { cn } from '@langgenius/dify-ui/cn'
import { Dialog, DialogContent, DialogTrigger } from '@langgenius/dify-ui/dialog'
import { Drawer, DrawerPopup, DrawerTrigger } from '@langgenius/dify-ui/drawer'
import { FieldControl, FieldLabel, FieldRoot } from '@langgenius/dify-ui/field'
import { Form } from '@langgenius/dify-ui/form'
import { Kbd, KbdGroup } from '@langgenius/dify-ui/kbd'
import { Popover, PopoverContent, PopoverTrigger } from '@langgenius/dify-ui/popover'
import { SegmentedControl, SegmentedControlItem } from '@langgenius/dify-ui/segmented-control'
import { Textarea } from '@langgenius/dify-ui/textarea'
import '@langgenius/dify-ui/styles.css' // once, in the app root
```

Importing from `@langgenius/dify-ui` (no subpath) is intentionally not supported — it keeps tree-shaking trivial and makes Storybook / test coverage attribution per-primitive.

## Primitives

| Category         | Subpath                                                                                                                                                                        | Notes                                                                                                 |
| ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------- |
| Actions          | `./button`                                                                                                                                                                     | Design-system CTA primitive with `cva` variants.                                                      |
| Controls         | `./segmented-control`                                                                                                                                                          | SegmentedControl for mode, filter, and view selection.                                                |
| Display          | `./collapsible`, `./kbd`                                                                                                                                                       | Collapsible disclosure primitive; keyboard input and shortcut keycap primitives.                      |
| Feedback         | `./meter`, `./toast`                                                                                                                                                           | Meter is inline status; Toast owns the `z-60` layer.                                                  |
| Form             | `./form`, `./field`, `./fieldset`, `./input`, `./textarea`, `./checkbox`, `./checkbox-group`, `./radio`, `./radio-group`, `./number-field`, `./select`, `./slider`, `./switch` | Native form boundary, field semantics, and controls.                                                  |
| Layout           | `./scroll-area`                                                                                                                                                                | Custom-styled scrollbar over the host viewport.                                                       |
| Media            | `./avatar`                                                                                                                                                                     | Avatar root, image, and fallback primitives.                                                          |
| Navigation       | `./file-tree`, `./pagination`, `./tabs`                                                                                                                                        | FileTree for preview-oriented file disclosure lists; Pagination for page navigation; Tabs for panels. |
| Overlay / menu   | `./alert-dialog`, `./context-menu`, `./dialog`, `./drawer`, `./dropdown-menu`, `./popover`, `./preview-card`, `./tooltip`                                                      | Portalled. See [Overlay & portal contract] below.                                                     |
| Search / pickers | `./autocomplete`, `./combobox`, `./select`                                                                                                                                     | Search input, searchable picker, and closed picker.                                                   |

Utilities:

- `./cn` — `clsx` + `tailwind-merge` wrapper. Use this for conditional class composition.
- `./styles.css` — the one CSS entry that ships the design tokens, theme variables, and project utilities/components. Import it once from the app root.

## Segmented control contract

`SegmentedControl` is Dify's design-system primitive for mode, filter, and view selection. It is built on Base UI `ToggleGroup` + `Toggle`, so use `Tabs` instead when the UI needs `tablist` / `tabpanel` semantics.

Its value contract follows Base UI: `value`, `defaultValue`, and `onValueChange` use arrays, and single-selection mode may report an empty array when the active item is toggled off.

## Form contract

Dify UI's form primitives are a Base UI composition layer for native form semantics, field accessibility, and design-system styling. They are intentionally not a form state-management framework. See the upstream [Base UI forms handbook], [Base UI Form], [Base UI Field], and [Base UI Fieldset] docs for the underlying component contracts.

Use `Form` for the submit boundary. It renders a native `<form>`, preserves Enter-to-submit and submit-button behavior, and adds Base UI's `onFormSubmit`, `errors`, `actionsRef`, and `validationMode` APIs for structured values and consolidated field validation. Prefer it over a bare `<form>` when the form is composed with Dify UI fields.

Use `FieldRoot` for each standalone named field. A field must have a stable `name`, a label relationship, and either a `FieldControl` or another control that participates in the same Base UI field context. Prefer a visible label for normal form rows; when the surrounding UI already supplies the visible text, use the matching label primitive visually hidden or put `aria-label` on the actual interactive control. `FieldDescription` and `FieldError` provide the message relationships that screen readers need, while the Dify wrapper adds the default Form Input Set styling from the design system.

Choose the label primitive by the control semantics. Text-like inputs, `Textarea`, input-based `Combobox` / `Autocomplete`, single `Checkbox` / `Radio`, `Switch`, and `NumberField` use `FieldLabel`. Trigger-based `Select` fields use `SelectLabel`; `Slider` fields use `SliderLabel`, with per-thumb `aria-label` only when the thumbs need distinct names. `SelectGroupLabel` and `AutocompleteGroupLabel` only label grouped options inside their popup content; they are not field labels.

Use `FieldsetRoot` and `FieldsetLegend` when one field is represented by a group of related controls, such as checkbox groups, radio groups, multi-thumb sliders, or a section that combines several inputs. For checkbox and radio groups, wrap each option with `FieldItem` and give each option its own label:

```tsx
<FieldRoot name="allowedNetworkProtocols">
  <FieldsetRoot render={<CheckboxGroup />}>
    <FieldsetLegend>Allowed network protocols</FieldsetLegend>
    <FieldItem>
      <FieldLabel className="flex items-center gap-2">
        <Checkbox value="https" />
        HTTPS
      </FieldLabel>
    </FieldItem>
  </FieldsetRoot>
</FieldRoot>
```

`FieldsetRoot` provides the group semantics and legend relationship. It does not own the interactive state of the grouped control. Pass `disabled`, `value`, `defaultValue`, and change handlers to the actual group primitive (`CheckboxGroup`, radio group, slider root, etc.) instead of relying on the fieldset wrapper to manage them.

For complex business forms, keep state ownership outside these primitives. TanStack Form, zod, server validation, dialog reset behavior, and schema-driven rendering belong to the feature layer in `web/`; they should pass `name`, `invalid`, `dirty`, `touched`, `value`, `onValueChange`, and errors into these primitives rather than replacing the field semantics. In this repo, `web/app/components/base/form` is the TanStack/schema runtime adapter; `packages/dify-ui` remains the primitive layer.

Migration rule for `web/`: if a UI has a save/submit action, do not leave it as unrelated `Input` and `Button` pieces. Give it a real submit boundary with `Form` or a native `<form>`, attach visible field names through the appropriate label primitive (`FieldLabel`, `SelectLabel`, `SliderLabel`, or `FieldsetLegend`), expose helper/error text through `FieldDescription` / `FieldError`, and keep non-submit buttons as `type="button"`.

## Tailwind CSS v4 integration

This package uses Tailwind CSS v4's CSS-first configuration model. Consumers should import Tailwind from their own root stylesheet, then import this package's CSS entry:

```css
@import 'tailwindcss';
@import '@langgenius/dify-ui/styles.css';
```

If a consumer uses Dify UI source files through the workspace, add an explicit source so Tailwind can detect utility classes:

```css
@source '../packages/dify-ui/src';
```

## Overlay & portal contract

Overlay primitives render their floating surfaces inside a [Base UI Portal] attached to `document.body`. This is the Base UI default — see the upstream [Portals][Base UI Portal] docs for the underlying behavior. Convenience content components such as `DialogContent`, `PopoverContent`, and `SelectContent` own their portal internally; primitives with explicit portal anatomy such as `Drawer` expose the matching `DrawerPortal` part so consumers can compose the full Base UI structure.

### Root isolation requirement

The host app **must** establish an isolated stacking context at its root so the portalled overlay layer is not clipped or re-ordered by ancestor `transform` / `filter` / `contain` styles. In the Dify web app this is done in `web/app/layout.tsx`:

```tsx
<body>
  <div className="isolate h-full">{children}</div>
</body>
```

Equivalent: any root element with `isolation: isolate` in CSS. Without it, overlays can be visually clipped on Safari when a descendant creates a new stacking context.

### z-index layering

Every overlay primitive uses a single, shared z-index. Do **not** override it at call sites.

| Layer                                                                                                               | z-index | Where                                                                      |
| ------------------------------------------------------------------------------------------------------------------- | ------- | -------------------------------------------------------------------------- |
| Overlays (Dialog, AlertDialog, Autocomplete, Combobox, Drawer, Popover, DropdownMenu, ContextMenu, Select, Tooltip) | `z-50`  | Positioner / Backdrop                                                      |
| Toast viewport                                                                                                      | `z-60`  | One layer above overlays so notifications are never hidden under a dialog. |

Rationale: Dify UI owns the normal application overlay layer. Overlay primitives share `z-50` and **rely on DOM order** for stacking — the portal mounted later wins. Toast owns `z-60` so notifications remain visible above dialogs, popovers, and other portalled surfaces without falling back to `z-9999`.

See `[web/docs/overlay.md](../../web/docs/overlay.md)` for the web app overlay best practices.

### Rules

- Never add ad hoc `z-*` overrides on primitives from this package. If something is getting clipped, fix the parent overlay structure instead of raising the child primitive.
- Never create an extra manual portal on top of our primitives — use the exported content / portal parts such as `DialogContent`, `PopoverContent`, and `DrawerPortal`. Base UI handles focus management, scroll-locking, and dismissal.
- When a primitive needs additional presentation chrome (e.g. a custom backdrop), add it **inside** the exported component, not at call sites.

### Tooltip, infotip, and popover semantics

- Use `Tooltip` only for short, non-interactive visual labels. The trigger must already have visible text or an `aria-label`; the tooltip is not the accessible name and must not contain links, buttons, forms, or structured prose.
- Use `Popover` for explanatory content, long text, rich layout, or anything users may need to reach on touch or with assistive technology. In `web/`, the `Infotip` wrapper is the preferred pattern for a `?` help glyph backed by `Popover`.
- Pick a `placement` and let the primitive own spacing. Avoid per-call-site offsets unless the component API explicitly needs a measured layout exception.
- When passing a Base UI trigger `render` prop, render a real `<button type="button">` for button-like triggers. If a Popover trigger must render a `div`, `span`, or another non-button element, pass `nativeButton={false}`.

## Development

- `pnpm -C packages/dify-ui test` — Vitest unit tests for primitives.
- `pnpm -C packages/dify-ui storybook` — Storybook on the default port. Each primitive has `index.stories.tsx`.
- `pnpm -C packages/dify-ui type-check` — `tsgo --noEmit` for this package only.

### Disabling Animations In Tests

Base UI can wait for `element.getAnimations()` to finish before it unmounts overlays, panels, and transition-driven components. Browser-based test runners can make that timing unstable, especially when tests assert final DOM state rather than animation behavior.

Set the Base UI test flag in a Vitest setup file to skip those waits:

```ts
(
  globalThis as typeof globalThis & {
    BASE_UI_ANIMATIONS_DISABLED: boolean
  }
).BASE_UI_ANIMATIONS_DISABLED = true
```

`packages/dify-ui/vitest.setup.ts` already applies this for primitive tests.

See `[AGENTS.md](./AGENTS.md)` for:

- Component authoring rules (one-component-per-folder, `cva` + `cn`, relative imports inside the package, subpath imports from consumers).
- Figma `--radius/`* token → Tailwind `rounded-*` class mapping.

## Not part of this package

- Application state (`jotai`, `zustand`), data fetching (`ky`, `@tanstack/react-query`, `@orpc/*`), i18n (`next-i18next` / `react-i18next`), and routing (`next`) all live in `web/`. This package has zero dependencies on them and must stay that way so it can eventually be consumed by other apps or extracted.
- Business components (chat, workflow, dataset views, etc.). Those belong in `web/app/components/...`.

[Base UI Field]: https://base-ui.com/react/components/field
[Base UI Fieldset]: https://base-ui.com/react/components/fieldset
[Base UI Form]: https://base-ui.com/react/components/form
[Base UI Portal]: https://base-ui.com/react/overview/quick-start#portals
[Base UI docs index]: https://base-ui.com/llms.txt
[Base UI forms handbook]: https://base-ui.com/react/handbook/forms
[Base UI]: https://base-ui.com/react
[Overlay & portal contract]: #overlay--portal-contract