# 읽어보기

- 원문 저장소: `langgenius/dify`
- 미러 저장소: `martinlee-git/dify`
- 원문 문서: https://github.com/langgenius/dify/blob/main/web/i18n-config/README.md
- 미러 경로: `web/i18n-config/README.md`

## 한글 요약

국제화(i18n) 소개 이 디렉토리에는 i18n 도구 및 구성이 포함되어 있습니다. 번역 파일은 web/i18n에 있습니다. 파일 구조 기본 언어는 영어를 사용합니다. 번역 파일은 언어별로 정리된 다음 모듈별로 정리됩니다. 예를 들어 앱 모듈의 영어 번역은 web/i18n/en US/app.json에 있습니다. 번역 파일은 플랫 키(점 표기법)가 포함된 JSON입니다. i18next는 keySeparator: false로 구성되므로 점이 키의 일부입니다. 네임스페이스는 camelCase 파일 이름(예: app debug.json appDebug)이므로 useTranslation('appDebug') 또는 t('key', { ns: 'appDebug' })를 사용합니다. 새 언어를 추가하거나 기존 번역을 수정하려면 언어 폴더에서 .json 파일을 생성하거나 업데이트하세요. 예를 들어, 프랑스어 번역을 추가하려는 경우 fr FR 새 폴더를 만들고 그 안에 번역 파일을 추가할 수 있습니다. 기본적으로 LanguagesSupported를 사용하여 지원되는 언어를 결정합니다. 예를 들어 로그인 페이지 및 설정에서

## 핵심 발췌

페이지에서는 LanguagesSupported를 사용하여 지원되는 언어를 결정하고 언어 선택 드롭다운에 표시합니다. 예 1. 새 언어에 대한 새 폴더를 만듭니다. 1. 새 폴더에서 번역 .json 파일을 수정합니다. 키를 플랫하게 유지합니다(예:Dialog.title). 1. 언어.ts 파일에 새 언어를 추가합니다. 1. 언어가 지원되는 경우 지원 필드를 true로 표시하는 것을 잊지 마십시오. 1. 때로는 서버 측에서 일부 변경을 수행해야 할 수도 있습니다. 이 파일도 변경해주세요. 👇 <https://github.com/langgenius/dify/blob/61e4bbabaf2758354db4073cbea09fdd21a5bec1/api/constants/언어s.py#L5 참고: I18nText 유형은 LanguagesSupported에서 자동으로 파생되므로 수동으로 유형을 추가할 필요가 없습니다. 정리 그게 다야! 새로운 l을 성공적으로 추가했습니다.

## 원문 내용

# Internationalization (i18n)

## Introduction

This directory contains i18n tooling and configuration. Translation files live under `web/i18n`.

## File Structure

```txt
web/i18n
├── en-US
│   ├── app.json
│   ├── app-debug.json
│   ├── common.json
│   └── ...
└── zh-Hans
    └── ...

web/i18n-config
├── language.ts
├── i18next-config.ts
└── ...
```

We use English as the default language. Translation files are organized by language and then by module. For example, the English translation for the `app` module is in `web/i18n/en-US/app.json`.

Translation files are JSON with flat keys (dot notation). i18next is configured with `keySeparator: false`, so dots are part of the key. The namespace is the camelCase file name (for example, `app-debug.json` -> `appDebug`), so use `useTranslation('appDebug')` or `t('key', { ns: 'appDebug' })`.

If you want to add a new language or modify an existing translation, create or update the `.json` files in the language folder.

For example, if you want to add French translation, you can create a new folder `fr-FR` and add the translation files in it.

By default we will use `LanguagesSupported` to determine which languages are supported. For example, in login page and settings page, we will use `LanguagesSupported` to determine which languages are supported and display them in the language selection dropdown.

## Example

1. Create a new folder for the new language.

```txt
cd web/i18n
cp -r en-US id-ID
```

1. Modify the translation `.json` files in the new folder. Keep keys flat (for example, `dialog.title`).

1. Add the new language to the `languages.ts` file.

```typescript
export const languages = [
  {
    value: 'en-US',
    name: 'English(United States)',
    example: 'Hello, Dify!',
    supported: true,
  },
  {
    value: 'zh-Hans',
    name: '简体中文',
    example: '你好，Dify！',
    supported: true,
  },
  {
    value: 'pt-BR',
    name: 'Português(Brasil)',
    example: 'Olá, Dify!',
    supported: true,
  },
  {
    value: 'es-ES',
    name: 'Español(España)',
    example: 'Saluton, Dify!',
    supported: false,
  },
  {
    value: 'fr-FR',
    name: 'Français(France)',
    example: 'Bonjour, Dify!',
    supported: false,
  },
  {
    value: 'de-DE',
    name: 'Deutsch(Deutschland)',
    example: 'Hallo, Dify!',
    supported: false,
  },
  {
    value: 'ja-JP',
    name: '日本語 (日本)',
    example: 'こんにちは、Dify!',
    supported: false,
  },
  {
    value: 'ko-KR',
    name: '한국어 (대한민국)',
    example: '안녕, Dify!',
    supported: true,
  },
  {
    value: 'ru-RU',
    name: 'Русский(Россия)',
    example: 'Привет, Dify!',
    supported: false,
  },
  {
    value: 'it-IT',
    name: 'Italiano(Italia)',
    example: 'Ciao, Dify!',
    supported: false,
  },
  {
    value: 'th-TH',
    name: 'ไทย(ประเทศไทย)',
    example: 'สวัสดี Dify!',
    supported: false,
  },
  {
    value: 'id-ID',
    name: 'Bahasa Indonesia',
    example: 'Halo, Dify!',
    supported: true,
  },
  {
    value: 'uk-UA',
    name: 'Українська(Україна)',
    example: 'Привет, Dify!',
    supported: true,
  },
  {
    value: 'fa-IR',
    name: 'Farsi (Iran)',
    example: 'سلام, دیفای!',
    supported: true,
  },
  {
    value: 'ar-TN',
    name: 'العربية (تونس)',
    example: 'مرحبا، Dify!',
    supported: true,
  },
  // Add your language here 👇
  // ...
  // Add your language here 👆
]
```

1. Don't forget to mark the supported field as `true` if the language is supported.

1. Sometimes you might need to do some changes in the server side. Please change this file as well. 👇
   <https://github.com/langgenius/dify/blob/61e4bbabaf2758354db4073cbea09fdd21a5bec1/api/constants/languages.py#L5>

> Note: `I18nText` type is automatically derived from `LanguagesSupported`, so you don't need to manually add types.

## Clean Up

That's it! You have successfully added a new language to the project. If you want to remove a language, you can simply delete the folder and remove the language from the `languages.ts` file.

We have a list of languages that we support in the `languages.ts` file. But some of them are not supported yet. So, they are marked as `false`. If you want to support a language, you can follow the steps above and mark the supported field as `true`.

## Utility scripts

- Check missing/extra keys: `pnpm run i18n:check --file app billing --lang zh-Hans [--auto-remove]`
  - Use space-separated values; repeat `--file` / `--lang` as needed. Returns non-zero on missing/extra keys; `--auto-remove` deletes extra keys automatically.

## Automatic Translation

Translation is handled automatically by Claude Code GitHub Actions. When changes are pushed to `web/i18n/en-US/*.json` on the main branch:

1. Claude Code analyzes the git diff to detect changes
1. Identifies three types of changes:
   - **ADD**: New keys that need translation
   - **UPDATE**: Modified keys that need re-translation (even if target language has existing translation)
   - **DELETE**: Removed keys that need to be deleted from other languages
1. Runs `i18n:check` to verify the initial sync status.
1. Translates missing/updated keys while preserving placeholders (`{{var}}`, `${var}`, `<tag>`) and removes deleted keys.
1. Runs `lint:fix` to sort JSON keys and `i18n:check` again to ensure everything is synchronized.
1. Creates a PR with the translations.

### Manual Trigger

To manually trigger translation:

1. Go to Actions > "Translate i18n Files with Claude Code"
1. Click "Run workflow"
1. Optionally configure:
   - **files**: Specific files to translate (space-separated, e.g., "app common")
   - **languages**: Specific languages to translate (space-separated, e.g., "zh-Hans ja-JP")
   - **mode**: `incremental` (default, only changes) or `full` (check all keys)

Workflow: `.github/workflows/translate-i18n-claude.yml`