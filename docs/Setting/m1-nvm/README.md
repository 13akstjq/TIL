---
title: M1 mac에 nvm설치하는 방법
description: 인텔맥과는 다른 M1 mac nvm 설치방법에 대해서 소개합니다.
date: 2022-11-13
sidebarDepth: 2
tag: Nextjs
thumbnail: https://user-images.githubusercontent.com/46010705/201511010-39dffa66-7acd-4fae-857a-5ed214f88ee8.png
---

# getStaticProps, getServerSideProps, getStaticPaths

::: tip 목표

Intel Mac에 nvm을 설치하는 방법을 M1 Mac에 하면 에러가 발생하는데,
M1 Mac에 nvm을 설치하는 방법에 대해 알 수 있습니다.

:::

<component is="script" src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-pub-4877378276818686" async></component>

<!-- vuepress 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="7889645103"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<component is="script">
(adsbygoogle = window.adsbygoogle || []).push({});
</component>

## 1. nvm 설치

```bash
brew install nvm
```

위와 같이 설치를 해주고 난 후에 인텔 Mac과 동일하게 nvm 환경변수 설정을 해주어야합니다.

```bash
mkdir ~/.nvm
```

여기가 인텔 Mac과 다른 부분인데요.
여러분 각자 사용하는 shell 설정 파일을 열어줍니다 .

```bash
vi ~/.bash_profile
vi ~/.zshrc // 저는 이거 사용
```

위에 열어둔 파일 맨 아래에 아래 변경된 설정을 복사 붙혀넣기 해줍니다.
**M1 Mac은 nvm 환경변수 설정하는 경로가 다르기 때문에 인텔 맥과 차이가 있습니다. **

```bash
export NVM_DIR="$HOME/.nvm"
  [ -s "/opt/homebrew/opt/nvm/nvm.sh" ] && \. "/opt/homebrew/opt/nvm/nvm.sh"  # This loads nvm
  [ -s "/opt/homebrew/opt/nvm/etc/bash_completion.d/nvm" ] && \. "/opt/homebrew/opt/nvm/etc/bash_completion.d/nvm"  # This loads nvm bash_completion
```

## 설치 확인

설정이 다 끝났다면 아래 명령어로 nvm이 잘 설치되었는지 확인해줍니다.

```
nvm -v
// 0.39.2
```

## 원하는 Node 버전 설치

```bash
nvm install 16.17.0
```

## 설치된 Node 버전 확인

```bash
nvm ls
```

<img src="./img/1.png" alt="" />

## 원하는 Node 버전으로 변경

```bash
nvm use 16.17.0
```
