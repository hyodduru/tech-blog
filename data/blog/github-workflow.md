---
title: '[TIL] github workflow를 통해 ci cd 개발 환경 구축하기'
date: '2023-06-09'
tags: ['ci cd', 'github workflow', '테스트 자동화', 'TIL']
draft: false
summary: '회사에서 이번에 새로운 프로젝트를 시작하게 되면서 레포를 새로 파고, 초기 세팅, 배포 등의 작업들을 하게 되었다. next js와 vercel로 배포 환경을 세팅하였는데, 알고보니 vercel에서는 github workflow를 통해 ci cd 환경에서 개발을 할 수 있는 방법을 제공하고 있었다.
'
images: ['/static/images/workflow.png']
---

회사에서 이번에 새로운 프로젝트를 시작하게 되면서 레포를 새로 파고, 초기 세팅, 배포 등의 작업들을 하게 되었다. next js와 vercel로 배포 환경을 세팅하였는데, 알고보니 vercel에서는 github workflow를 통해 ci cd 환경에서 개발을 할 수 있는 방법을 제공하고 있었다.

내가 참고한 자료 : https://vercel.com/guides/how-can-i-use-github-actions-with-vercel

이를 참고해서 workflow를 세팅할 수 있었는데 생각보다 간단하고 쉽게 따라할 수 있어서 공유해보려고 한다.

사실 별거 없고 공식 문서에서 하라는 대로 작업해주면 된다.

## github workflow를 통해 ci cd 개발 환경 구축하기

### 1. vercel로 배포를 하고나면, github workflows 폴더 내에 파일을 생성한다.

`./github/workflows/frontend-stag.yml` 과 같이 workflows 폴더 내에 yml 파일을 만들어주면 된다.
스크립트는 공식 문서에 아주 친절하게 나와있다. 예시를 보여주면,

```yml
#  github action 이름
name: Frontend Stag

# github settings - Secrets and Variables 에서 설정해주는 key
env:
  VERCEL_ORG_ID: ${{ secrets.EXAMPLE_ID }}
  VERCEL_PROJECT_ID: ${{ secrets.EXAMPLE_PROJECT_ID }}
  NODE_OPTIONS: --max_old_space_size=4096

on:
  push:
    branches:
      - main
    #   workflow를 적용할 path
    paths:
      - 'frontend/**'
      - .github/workflows/frontend-stag.yml

jobs:
  install-dependencies:
    runs-on: ubuntu-latest
    defaults:
      run:
        working-directory: frontend
    # ci cd 순서
    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Setup node.js
        uses: actions/setup-node@v3
        with:
          node-version: '16'

      - name: Install dependencies
        run: yarn install --immutable --immutable-cache
  # 위의 install-dependencies가 실행이 된 후 성공했을 때에만 lint를 돌린다는 의미
  lint:
    runs-on: ubuntu-latest
    needs: [install-dependencies]
    defaults:
      run:
        working-directory: frontend
    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Setup node.js
        uses: actions/setup-node@v3
        with:
          node-version: '16'

      - name: Install dependencies
        run: yarn install --immutable --immutable-cache

      - name: Lint
        run: yarn lint

  unit-testing:
    permissions:
      checks: write
      pull-requests: write
      contents: write
    runs-on: ubuntu-latest
    needs: [install-dependencies]
    defaults:
      run:
        working-directory: frontend
    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Setup node.js
        uses: actions/setup-node@v3
        with:
          node-version: '16'

      - name: Install dependencies
        run: yarn install --immutable --immutable-cache

      - name: Unit testing
        run: |
          yarn test \
            --ci \
            --runInBand \
            --coverage \
            --watchAll=false \
            --passWithNoTests \
            --json \
            --testLocationInResults \
            --outputFile=report.json
      - name: Report unit test coverage
        uses: ArtiomTr/jest-coverage-report-action@v2.2.4
        with:
          working-directory: ./frontend
          package-manager: yarn
          skip-step: all
          coverage-file: ./report.json

  deploy:
    concurrency: frontend-stag-deploy
    runs-on: ubuntu-latest
    needs: [lint, unit-testing]
    steps:
      - uses: actions/checkout@v3

      - name: Install Vercel CLI
        run: npm install --global vercel@latest

      - name: Pull Vercel Environment Information
        run: |
          vercel pull --yes --environment=preview --token=${{ secrets.EXAMPLE_VERCEL_USER_TOKEN }} --scope=example
          cp .vercel/.env.preview.local frontend/.env
      - name: Build Project Artifacts
        run: vercel build --token=${{ secrets.EXAMPLE_VERCEL_USER_TOKEN }}

      - name: Deploy Project Artifacts to Vercel
        run: vercel deploy --prebuilt --token=${{ secrets.EXAMPLE_VERCEL_USER_TOKEN }} > domain.txt

      - name: Vercel alias domain
        run: vercel alias --token=${{ secrets.EXAMPLE_VERCEL_USER_TOKEN }} set `cat domain.txt` EXAMPLE.example.net --scope=example
        # example.net 에는 해당하는 도메인 주소를 적어주면 된다.
```

주석에 부가 설명을 달아놓았다.

위의 명령어들을 실행하고, 코드 상 문제가 없을 때 마지막 단계인 deploy까지 성공하게 되고, 배포가 되는 구조이다.

## 2. 위의 파일을 작업하고 있는 레파지토리에 올린다.

해당 작업 내용을 담은 PR을 올리면, 아래와 같이 자동으로 ci cd action이 돈다.

![github-workflow](/static/images/workflow.png)

스크립트에서 작성한 모든 단계를 통과했을 때 배포가 자동으로 되는 것이다.

해당 pr을 merge하면, stag 혹은 production에서 build가 되고, 자동으로 배포가 된다.

**꼭 vercel에서 뿐 아니라 다른 배포 환경에서도 ci cd 환경은 구축할 수 있으니, 각 배포 공식 문서를 참고하면 될 것 같다.**

따로 일일히 확인하지 않아도, 알아서 배포가 가능한 형태인지를 확인해주어, 시간도 절약되고, 유지보수에도 좋아서 알아두면 정말 유용한 것 같아요~!
