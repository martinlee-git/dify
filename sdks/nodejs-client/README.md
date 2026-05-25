# 읽어보기

- 원문 저장소: `langgenius/dify`
- 미러 저장소: `martinlee-git/dify`
- 원문 문서: https://github.com/langgenius/dify/blob/main/sdks/nodejs-client/README.md
- 미러 경로: `sdks/nodejs-client/README.md`

## 한글 요약

Dify Node.js SDK Dify API를 위한 Node.js SDK로, Dify를 Node.js 애플리케이션에 쉽게 통합할 수 있습니다. 설치 사용법 SDK를 설치한 후 프로젝트에서 다음과 같이 사용할 수 있습니다. 참고: 앱 엔드포인트는 앱 API 토큰을 사용합니다. 기술 자료 및 작업 영역 끝점은 데이터 세트 API 토큰을 사용합니다. 채팅/완료에는 요청 페이로드에 안정적인 사용자 식별자가 필요합니다. 스트리밍 응답의 경우 반환된 AsyncIterable을 반복합니다. 텍스트를 수집하려면 stream.toText()를 사용하세요. 유지 관리자 이 패키지는 저장소 작업 공간에서 게시됩니다. pnpm 설치를 통해 리포지토리 루트에서 종속성을 설치한 다음 연습 실행 및 게시를 위해 ./scripts/publish.sh를 사용하여 카탈로그: 릴리스 전에 종속성을 해결합니다. 라이센스 이 SDK는 MIT 라이센스에 따라 출시됩니다.

## 핵심 발췌

Dify Node.js SDK Dify API를 위한 Node.js SDK로, Dify를 Node.js 애플리케이션에 쉽게 통합할 수 있습니다. ## 설치 ## SDK 설치 후 사용법

## 원문 내용

# Dify Node.js SDK

This is the Node.js SDK for the Dify API, which allows you to easily integrate Dify into your Node.js applications.

## Install

```bash
npm install dify-client
```

## Usage

After installing the SDK, you can use it in your project like this:

```js
import {
  DifyClient,
  ChatClient,
  CompletionClient,
  WorkflowClient,
  KnowledgeBaseClient,
  WorkspaceClient
} from 'dify-client'

const API_KEY = 'your-app-api-key'
const DATASET_API_KEY = 'your-dataset-api-key'
const user = 'random-user-id'
const query = 'Please tell me a short story in 10 words or less.'

const chatClient = new ChatClient(API_KEY)
const completionClient = new CompletionClient(API_KEY)
const workflowClient = new WorkflowClient(API_KEY)
const kbClient = new KnowledgeBaseClient(DATASET_API_KEY)
const workspaceClient = new WorkspaceClient(DATASET_API_KEY)
const client = new DifyClient(API_KEY)

// App core
await client.getApplicationParameters(user)
await client.messageFeedback('message-id', 'like', user)

// Completion (blocking)
await completionClient.createCompletionMessage({
  inputs: { query },
  user,
  response_mode: 'blocking'
})

// Chat (streaming)
const stream = await chatClient.createChatMessage({
  inputs: {},
  query,
  user,
  response_mode: 'streaming'
})
for await (const event of stream) {
  console.log(event.event, event.data)
}

// Chatflow (advanced chat via workflow_id)
await chatClient.createChatMessage({
  inputs: {},
  query,
  user,
  workflow_id: 'workflow-id',
  response_mode: 'blocking'
})

// Workflow run (blocking or streaming)
await workflowClient.run({
  inputs: { query },
  user,
  response_mode: 'blocking'
})

// Knowledge base (dataset token required)
await kbClient.listDatasets({ page: 1, limit: 20 })
await kbClient.createDataset({ name: 'KB', indexing_technique: 'economy' })

// RAG pipeline (may require service API route registration)
const pipelineStream = await kbClient.runPipeline('dataset-id', {
  inputs: {},
  datasource_type: 'online_document',
  datasource_info_list: [],
  start_node_id: 'start-node-id',
  is_published: true,
  response_mode: 'streaming'
})
for await (const event of pipelineStream) {
  console.log(event.data)
}

// Workspace models (dataset token required)
await workspaceClient.getModelsByType('text-embedding')

```

Notes:

- App endpoints use an app API token; knowledge base and workspace endpoints use a dataset API token.
- Chat/completion require a stable `user` identifier in the request payload.
- For streaming responses, iterate the returned AsyncIterable. Use `stream.toText()` to collect text.

## Maintainers

This package is published from the repository workspace. Install dependencies from the repository root with `pnpm install`, then use `./scripts/publish.sh` for dry runs and publishing so `catalog:` dependencies are resolved before release.

## License

This SDK is released under the MIT License.