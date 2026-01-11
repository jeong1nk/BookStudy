## 🔖핵심 키워드
- **MCP**: AI 모델이 외부 서비스와  연결되는 표준 통신 규약
- **로컬 MCP**: 내 컴퓨터 안에서 직접 실행하는 MCP
- **원격 MCP**: 인터넷 상 외부서버에서 실행하는 MCP
- **Playwright**: 웹 자동 테스트 도구
- **버전 관리**: 코드 변경 이력을 기록하고 관리
- **배포**: 앱을 외부 서버에 올리는 과정
- **Vercel**: 자동 빌드/배포 클라우드 플랫 폼
- **Superbase**: 클라우드 데이터베이스 플랫폼

---

## **08-1 MCP 이해하고 클로드 코드와 연결하기**
### MCP(Model Context Protocol)란?
- AI 모델이 외부 데이터, 도구, 서비스와 연결될 때 사용하는 표준화된 통신 규약
- AI가 다양한 시스템과 일관된 방식으로 대화하고 협업할 수 있게 해주는 다리 역할
- **클로드 코드에 특별한 기능들을 연결해주는 연결 장치**이다.

### MCP 서버
#### 로컬 MCP
- MCP 중계 프로그램을 내 컴퓨터 안에서 직접 실행
- 인터넷 없이 작동  / 빠른 응답 속도 / 개인정보 보호 유리

```jsx
##터미널에서
claude mcp add [이름] -s local -- [실행 명령]
```

- **명령 프롬포트(cmd)에서 실행하는 경우: (폴더에서 주소란에 cmd 입력)**

```jsx
claude mcp add (MCP이름) -s local -- cmd /c npx -y @(어느 팀에서 만들었는지)/(이 MCP의 이름)@(버전)
```

- **Windows  PowerSehell에서 실행하는 경우: (폴더에서  오른쪽 마우스 - 명령 프롬포트 실행)**

```jsx
claude mcp add (MCP이름) -s local -- npx @(어느 팀에서 만들었는지)/(이 MCP의 이름)@(버전)
```

- `-s(--scope)` : 어느 범위까지 이 MCP를 사용할지(local, project, user)

#### 원격 MCP
- 인터넷상 외부 서버에서 실행
- 설정이 간편 / 여러 사용자와 공유 용이 / 명령어 한 줄로 연결
- 인증 과정이 필요

```jsx
##터미널에서
claude mcp add --transport http [이름] [서버URL]
```

### API vs MCP 차이점
- API: 서로 다른 프로그램이 데이터를 주고 받을 수 있게 하는 인터페이스. 다른 **서비스의 기능을 잠시 빌려오는 방식**
    - 예) 오픈라우터 API로 AI 모델을 가져와 텍스트 생성에 사용
- MCP: 여러 API와 파일,  데이터베이스를  한꺼번에 통합해 연결하는 프로토콜. AI가 외부 시스템에 직접 접근 가능
    - 예) AI가 직접 파일 시스템 조작, 데이터베이스 조회/수정

### 노션 MCP 연동하기(원격 MCP)
1. 클로드 코드 실행 후 /mcp 명령어로 목록 확인→모션 계정 인증
2. 클로드 코드에서 노션 MCP 기능 사용(15개 도구 제공)

```jsx
claude mcp add -transport http notion https://mcp.notion.com/mcp
```

- 예) “클로드 코드의 최신 업데이트를 검색하고, 노션에 요약해서 저장해줘”

### /mcp
- MCP를 관리할 수 있는 명령어, 현재 연동된 MCP 정보 확인 가능
- 🔖클로드 코드 내에서 페이지 열기 : ctrl + 클릭

## 08-2 MCP로 구현하는 완전 자동화 개발 환경
### 자동화를 위한 MCP 예제
- **Context7**: 클로드 코드가 항상 최신 문서를 참고하도록 돕는 지식 연결 도구
    - API 키 필요
    - https://github.com/upstash/context7
    - https://context7.com/

```jsx
"context7 mcp를 이용해 (내가 만든 앱)이 최신 버전의 코드를 사용했는지 점검해줘"
```

- **Playwright**: 웹사이트를 자동으로 테스트하고 UI를 검증하는 브라우저 제어 도구
    - https://github.com/microsoft/playwright-mcp
- **GitHub**: 프로젝트를 버전관리하고 자동으로 업로드하는 협업도구
    - API 키 필요
    - https://github.com/github/github-mcp-server

```jsx
"현재 폴더에 만들어진 (내가 만든 앱)을 깃허브에 저장하고 싶어. github mcp를 사용해서 (저장할 저장소 이름 지)이라는 저장소를 만들고 업로드해 줘."
```

- 🔖**MCP 서버 찾는 방법**: 구글에서 “[서비스명] MCP” 검색 → 깃허브 문서 확인
- 에러가 생길 경우: `claude MCP GET` 입력해 해당 MCP의 정보만 따로 볼 수 있음

## 08-3 데이터베이스 연결해 진짜 서비스 만들기
### 배포(Deploy)란?
- 내 어플리케이션을 외부의 인터넷에 연결된 제3의 서버에 올리는 과정
- 항상 켜져있는 서버에서 앱을 실행해 언제, 어디서든 URL로 접속 가능
- 깃허브 저장소 준비 → Vercel에 깃허브  인증 → 즉시 URL 발급

### Vercel
- 작성한 코드를 깃허브에 올리면 자동으로 빌드, 배포 해주는 클라우드 플랫폼
- 간단한 웹 어플리케이션은 무료로 배포 가능

### 로컬 스토리지 vs 클라우드 스토리지
- 로컬 스토리지의 한계
    - 다른 컴퓨터 / 브라우저에서 데이터 공유 안됨
    - 브라우저 캐시 삭제 시 데이터 사라짐
    - 기기를 바꾸면 데이터 유실
    - 시크릿 모드에서 데이터 보이지 않음
- 클라우드 스토리지의 장점
    - 어디서나 동일한 데이터 접근 가능
    - 여러  기기에서 같은 리스트 확인
    - 여러 사람이 공유/협업 가능
    - 데이터 영구 보존

### **Supabase로 클라우드 데이터베이스 연동**
- 데이터베이스 구축을 자동화해주는 클라우드 데이터베이스 플랫폼
- 무료 플랜 제공 / API 자동 생성 / 실시간 동기화 / 다중 사용자 지원
- 🔖앱에서 아이템 추가 → DB 테이블에 행 추가, 체크박스 클릭 → completed 열이 TRUE로 변경 → 다른 기기에서도 동일한 데이터 확인 가능
- **단, Free 버전의 경우 프로젝트를 두개까지 만들 수 있으며, 올라간 데이터를  일주일 마다 로그인해 Resume 해줘야 함**

- Windows 명령 프롬프트(cmd)에서 실행하는 경우:

```
claude mcp add supabase -s local -e SUPABASE_ACCESS_TOKEN=<Supabase API 토큰> -- cmd /c npx -y @supabase/mcp-server-supabase@latest
```

- Windows PowerShell에서 실행하는 경우:

```
claude mcp add --transport http supabase "https://mcp.supabase.com/mcp"
```
