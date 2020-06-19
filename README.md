# Serverlss Framework - Lambda 개발 환경 샘플

## 사전 환경 구축

- Webstorm 설치

- node@12 설치
    - $ brew install node@12
    - $ echo 'export PATH="/usr/local/opt/node@12/bin:$PATH"' >> ~/.zshrc

- serverless framework 설치
    - $ curl -o- -L https://slss.io/install | bash

- aws profile setting
    - aws iam에서 발급받은 accessKey, secret으로 로컬에 프로파일을 세팅한다.
    - $ brew install awscli
    - $ aws configure --profile idn
        AWS Access Key ID [None]: AKIAVFXXXXXXXQUD2CPSR<br>
        AWS Secret Access Key [None]: 8bJTW21dlFXXXXXXXXXXXpAy2g9utgUGS7i<br> 
        Default region name [None]: ap-southeast-1<br>
        Default output format [None]: json

        위 내용을 세팅하면 idn profile이 하나 생성된다. 동일한 방식으로 나머지 국가도 프로필을 세팅한다.

    - 대만)
        - $ aws configure --profile twn
    - 태국)
        - $ aws configure --profile tha

    - 세팅한 정보는 ~/.aws 디렉터리 하위의 config, credentials에 나뉘어 저장된다.

## 개발 프로젝트 세팅

### 신규 프로젝트를 생성하는 경우
    - 서버리스 프로젝트 생성
    - $ sls create --template aws-nodejs --path [생성할 디렉터리명] --name [서비스이름]
    ex)
    - $ sls create --template aws-nodejs --path serverless-lambda-sample --name sample

    - node 환경 setup
        - 프로젝트 root에서 다음 명령어 실행
            - $ npm init
            - $ npm i aws-sdk
            - $ npm i mysql
            - $ npm i redis

    - serverless plugin setup
        - prune-plugin : 최근 배포된 내역 n개만 유지하고 과거 버전을 자동으로 삭제해 주는 plugin
        - $ sls plugin install -n serverless-prune-plugin

### 기존 프로젝트를 불러오는 경우
    - Git Repository Clone
    - Webstorm -> VCS - Get from version Control
    - 소스 다운로드 완료 후 프로젝트 root에서 다음 명령어 실행
    - $ npm install

#### 프로젝트 디렉터리 구조
    serverless-lambda-sample
    ├── README.md
    ├── handler.js
    ├── node_modules
    ├── package-lock.json
    ├── package.json
    └── serverless.yml
    
- handler.js : 비즈니스 로직을 작성하는 파일
- serverless.yml : 프로젝트 환경 정보를 세팅하는 파일
- 기타 디렉터리 및 파일은 node환경을 구성하면서 자동으로 만들어짐

### 국가별 환경 분리

- 환경 변수 분리
    - 프로젝트 root에 env 디렉터리 생성. env하위에 국가별로 디렉터리 생성 idn/twn/tha
    - 국가별 디렉터리 안에 환경.json 생성

#### 분리 후 프로젝트 디렉터리 구조
    serverless-lambda-sample
    ├── README.md
    ├── env
    │   ├── idn
    │   │   ├── local.json
    │   │   └── sandbox.json
    │   ├── tha
    │   │   ├── local.json
    │   │   └── sandbox.json
    │   └── twn
    │       ├── local.json
    │       └── sandbox.json
    ├── handler.js
    ├── node_modules
    ├── package-lock.json
    ├── package.json
    ├── serverless.yml

### 설정 파일 분리
    - serverless.yml을 국가별로 분리하여 생성한다.
        - serverless_idn.yml
        - serverless_twn.yml
        - serverless_tha.yml 생성

#### 분리 후 프로젝트 디렉터리 구조
    serverless-lambda-sample
    ├── README.md
    ├── env
    │   ├── idn
    │   │   ├── local.json
    │   │   └── sandbox.json
    │   ├── tha
    │   │   ├── local.json
    │   │   └── sandbox.json
    │   └── twn
    │       ├── local.json
    │       └── sandbox.json
    ├── handler.js
    ├── node_modules
    ├── package-lock.json
    ├── package.json
    ├── serverless.yml
    ├── serverless_idn.yml
    ├── serverless_tha.yml
    └── serverless_twn.yml

### 테스트
    $ sls invoke local -c [serverless_idn.yml/serverless_twn.yml/serverless_tha.yml] -f hello

### 원격 서버 배포
    $ sls deploy -s [dev/sandbox/production] -c [serverless_idn.yml/serverless_twn.yml/serverless_tha.yml]

### 참고
    - Serverless Quick Start
        - https://www.serverless.com/framework/docs/providers/aws/guide/quick-start/
    - Serverless.yml Full Reference
        - https://www.serverless.com/framework/docs/providers/aws/guide/serverless.yml/
