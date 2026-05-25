# 읽어보기

- 원문 저장소: `langgenius/dify`
- 미러 저장소: `martinlee-git/dify`
- 원문 문서: https://github.com/langgenius/dify/blob/main/docker/certbot/README.md
- 미러 경로: `docker/certbot/README.md`

## 한글 요약

SSL 인증서를 사용하여 새 서버 시작 간단한 설명 이전 버전과 호환되는 docker compose certbot 구성(certbot 컨테이너 없음).\ 이 기능을 사용하려면 docker compose profile certbot up을 사용하세요. SSL 인증서로 새 서버를 시작하는 가장 간단한 방법 1. letsencrypt certs\ set .env 값 실행 명령 받기: 컨테이너가 시작된 후: 1. .env 파일을 편집하고 docker compose profile certbot을 다시 실행합니다.\ set .env value 추가로 명령 실행: 그런 다음 HTTPS를 사용하여 서버에 액세스할 수 있습니다.\ https://your domain.com SSL 인증서 갱신 SSL 인증서 갱신의 경우 아래 명령을 실행합니다. certbot에 대한 옵션 CERTBOT OPTIONS 키가 도움이 될 수 있습니다. 테스트. 즉, CERTBOT OPTIONS에 변경 사항을 적용하려면 인증서를 업데이트하기 전에 certbot 컨테이너를 다시 생성하십시오. 그런 다음 필요한 경우 nginx 컨테이너를 다시 로드합니다. 레거시 서버의 경우 이전과 같이 nginx/ssl 디렉토리의 인증서 파일을 사용하려면 프로필 certbot 옵션 없이 컨테이너를 시작하기만 하면 됩니다.

## 핵심 발췌

SSL 인증서를 사용하여 새 서버 시작 ## 간단한 설명 docker compose certbot 구성은 이전 버전과 호환됩니다(certbot 컨테이너 없음).\ 이 f를 사용하려면 docker compose profile certbot을 사용하세요.

## 원문 내용

# Launching new servers with SSL certificates

## Short description

docker compose certbot configurations with Backward compatibility (without certbot container).\
Use `docker compose --profile certbot up` to use this features.

## The simplest way for launching new servers with SSL certificates

1. Get letsencrypt certs\
   set `.env` values
   ```properties
   NGINX_SSL_CERT_FILENAME=fullchain.pem
   NGINX_SSL_CERT_KEY_FILENAME=privkey.pem
   NGINX_ENABLE_CERTBOT_CHALLENGE=true
   CERTBOT_DOMAIN=your_domain.com
   CERTBOT_EMAIL=example@your_domain.com
   ```
   execute command:
   ```shell
   docker network prune
   docker compose --profile certbot up --force-recreate -d
   ```
   then after the containers launched:
   ```shell
   docker compose exec -it certbot /bin/sh /update-cert.sh
   ```
1. Edit `.env` file and `docker compose --profile certbot up` again.\
   set `.env` value additionally
   ```properties
   NGINX_HTTPS_ENABLED=true
   ```
   execute command:
   ```shell
   docker compose --profile certbot up -d --no-deps --force-recreate nginx
   ```
   Then you can access your serve with HTTPS.\
   [https://your_domain.com](https://your_domain.com)

## SSL certificates renewal

For SSL certificates renewal, execute commands below:

```shell
docker compose exec -it certbot /bin/sh /update-cert.sh
docker compose exec nginx nginx -s reload
```

## Options for certbot

`CERTBOT_OPTIONS` key might be helpful for testing. i.e.,

```properties
CERTBOT_OPTIONS=--dry-run
```

To apply changes to `CERTBOT_OPTIONS`, regenerate the certbot container before updating the certificates.

```shell
docker compose --profile certbot up -d --no-deps --force-recreate certbot
docker compose exec -it certbot /bin/sh /update-cert.sh
```

Then, reload the nginx container if necessary.

```shell
docker compose exec nginx nginx -s reload
```

## For legacy servers

To use cert files dir `nginx/ssl` as before, simply launch containers WITHOUT `--profile certbot` option.

```shell
docker compose up -d
```