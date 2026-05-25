# 읽어보기

- 원문 저장소: `langgenius/dify`
- 미러 저장소: `martinlee-git/dify`
- 원문 문서: https://github.com/langgenius/dify/blob/main/sdks/php-client/README.md
- 미러 경로: `sdks/php-client/README.md`

## 한글 요약

Dify PHP SDK 이것은 Dify API를 위한 PHP SDK로, Dify를 PHP 애플리케이션에 쉽게 통합할 수 있습니다. 요구 사항 PHP 7.2 이상 Guzzle HTTP 클라이언트 라이브러리 사용법 예제를 시도하려면 이 디렉터리에서 Composer install을 실행하면 됩니다. 기존 프로젝트에서 dify client.php를 프로젝트에 복사하고 다음 항목을 작곡가.json 파일에 병합한 후 작곡가 설치 && 작곡가 덤프 자동 로드를 실행하여 설치합니다. Guzzle에는 7.9가 필요하지 않으며 다른 버전은 테스트되지 않았지만 시도해 볼 수 있습니다. SDK를 설치한 후 다음과 같이 프로젝트에서 사용할 수 있습니다. '여기에 있는 API 키'를 실제 Dify API 키로 바꾸세요. 라이센스 이 SDK는 MIT 라이센스에 따라 출시됩니다.

## 핵심 발췌

Dify PHP SDK 이것은 Dify API를 위한 PHP SDK로, Dify를 PHP 애플리케이션에 쉽게 통합할 수 있습니다. ## 요구사항 PHP 7.2 이상 Guzzle HTTP 클라이언트 라이브러리 ## 사용법 사용해보고 싶다면

## 원문 내용

# Dify PHP SDK

This is the PHP SDK for the Dify API, which allows you to easily integrate Dify into your PHP applications.

## Requirements

- PHP 7.2 or later
- Guzzle HTTP client library

## Usage

If you want to try the example, you can run `composer install` in this directory.

In exist project, copy the `dify-client.php` to you project, and merge the following to your `composer.json` file, then run `composer install && composer dump-autoload` to install. Guzzle does not require 7.9, other versions have not been tested, but you can try.

```json
{
    "require": {
        "guzzlehttp/guzzle": "^7.9"
    },
    "autoload": {
        "files": ["path/to/dify-client.php"]
    }
}
```

After installing the SDK, you can use it in your project like this:

```php
<?php

require 'vendor/autoload.php';

$apiKey = 'your-api-key-here';

$difyClient = new DifyClient($apiKey);

// Create a completion client
$completionClient = new CompletionClient($apiKey);
$response = $completionClient->create_completion_message(array("query" => "Who are you?"), "blocking", "user_id");

// Create a chat client
$chatClient = new ChatClient($apiKey);
$response = $chatClient->create_chat_message(array(), "Who are you?", "user_id", "blocking", $conversation_id);

$fileForVision = [
    [
        "type" => "image",
        "transfer_method" => "remote_url",
        "url" => "your_image_url"
    ]
];

// $fileForVision = [
//     [
//         "type" => "image",
//         "transfer_method" => "local_file",
//         "url" => "your_file_id"
//     ]
// ];

// Create a completion client with vision model like gpt-4-vision
$response = $completionClient->create_completion_message(array("query" => "Describe this image."), "blocking", "user_id", $fileForVision);

// Create a chat client with vision model like gpt-4-vision
$response = $chatClient->create_chat_message(array(), "Describe this image.", "user_id", "blocking", $conversation_id, $fileForVision);

// File Upload
$fileForUpload = [
    [
        'tmp_name' => '/path/to/file/filename.jpg',
        'name' => 'filename.jpg'
    ]
];
$response = $difyClient->file_upload("user_id", $fileForUpload);
$result = json_decode($response->getBody(), true);
echo 'upload_file_id: ' . $result['id'];

// Fetch application parameters
$response = $difyClient->get_application_parameters("user_id");

// Provide feedback for a message
$response = $difyClient->message_feedback($message_id, $rating, "user_id");

// Other available methods:
// - get_conversation_messages()
// - get_conversations()
// - rename_conversation()
```

Replace 'your-api-key-here' with your actual Dify API key.

## License

This SDK is released under the MIT License.