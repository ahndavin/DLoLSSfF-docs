# DLoLSSfF
> **Discord-based League of Legends Support System for Friends**

매일 나와 함께 게임하는 친구들(일부는 실존 여부가 불확실하지만, 일단 존재한다고 가정)을 위한 디스코드용 LoL 서포트 시스템 제작 프로젝트

<span style="float:right;">Developed by <span style="color:green">**Si vales valeo#EUW**</span></span>

## 소개
- 이 프로젝트는 LoL 클라이언트 이벤트와 외부 API를 연결한다.
- 주요 기능은 매치 수락 자동화, 챔피언 선택 단계 분석, 경기 종료 후 리포트 생성이다.
- 결과물은 디스코드 채널로 전달된다.

## 가치
- 불필요한 단계를 자동화함으로써 게임 시작 전 정비시간을 확보한다.
- 팀 조합 정보를 구조화해 픽/운영 판단 속도를 높인다.
- 경기 기록을 누적해 팀 단위 회고 품질을 개선한다.

## 기능
### v1.0.0 범위
- **매치 자동 수락**: 매치 검색 단계에서 수락 버튼을 자동 처리한다.
- **팀 조합 분석**: 챔피언 선택 종료 직후 아군/적군 조합의 강점·약점과 운영 팁을 생성한다.
- **매치 리포트**: 경기 종료 후 MVP/MuVP를 포함한 요약 리포트를 생성한다.

### v2.0.0 예정
- **정기 팀 리포트**: 주간 단위 팀 또는 개인 성향 분석 리포트를 자동 생성한다.

### v3.0.0 예정
- **상대 픽 대응 추천**: 상대 확정 픽 기반 라인 대응 챔피언을 제안한다.
- **팀 시너지 추천**: 아군 확정 픽 기반 시너지 챔피언을 제안한다.

> 버전 정책: `x.y.z` (`x`: 대규모 기능 추가, `y`: 클라이언트/연동 계층 변경, `z`: 서버/백엔드 변경)

## 아키텍처
<img src="./Docs_Dev/Architecture_Diagram.png" width="80%" height="80%">

### 시스템 구성
- **LCU Agent**: League Client Update API(LCU API) 기반 로컬 에이전트.
- **Server PC**: Raspberry Pi 기반 상시 실행 서버.
- **Discord Webhook**: 분석 결과 전달 채널.
- **Database (SQLite 예정)**: 경기 데이터 및 리포트 생성용 저장소.

### 동작 흐름
1. LCU Agent가 게임 상태 이벤트를 감지한다.
2. 서버가 이벤트를 검증하고 Riot API 데이터 수집을 수행한다.
3. 분석 파이프라인이 결과를 생성한다.
4. Discord Webhook이 결과를 게시한다.

## 운영 제약 및 정책
- **중복 실행 방지**: 동일 게임 기준으로 트리거를 1회만 허용한다.
- **허용 큐 제한**: 솔로 랭크(420), 자유 랭크(440)만 지원한다.
- **인증 Allowlist**: 사전 등록된 플레이어(Agent Player)만 사용권한을 갖는다.
- **밴/픽 단계 트리거 조건**: 챔피언 "호버" 상태는 제외하고 "확정 픽" 이벤트만 처리한다.
- **조합 분석 트리거 조건**: 10인 챔피언 픽이 모두 확정된 경우에만 실행한다.

## 데이터 및 분석
- 경기 종료 직전 `PreEndOfGame` 이벤트에서 `game_id`를 서버로 전달한다.
- 서버는 일정 지연 후 Riot API에서 매치/타임라인 데이터를 수집한다.
- 분석 파이프라인은 비용과 품질 균형을 위해 다중 모델 전략을 사용한다.

## 기술 스택
- **Language**: Python 3.13
- **AI**: OpenAI API
- **Discord**: discord.py
- **HTTP**: FastAPI
- **DB**: SQLite (스키마 설계 예정)

## 용어집
- **LCU API**: League Client Update API. 로컬 LoL 클라이언트 제어/조회 인터페이스.
- **Agent Player**: 시스템 기능을 사용할 수 있는 인증된 플레이어.
- **Team Composition (팀 조합)**: 아군 또는 양 팀 챔피언 조합.
- **Match Report (매치 리포트)**: 경기 종료 후 생성되는 요약 분석 문서.
- **Gameflow (게임 흐름)**: 초반/중반/후반 구간별 운영 전개.
- **Counter Pick (상대 대응 픽)**: 상대 조합/라인에 대응하기 위한 추천 챔피언.
- **Synergy Pick (시너지 픽)**: 아군 조합과 조합 효율이 높은 추천 챔피언.

## Thanks to
- **Bami on bush#BAMI** <span style="float:right;">(재검증 필요)</span>
    - 팀 조합 및 경기력 평가 기준을 제시함
- **Charmcharmcharm#EUW** <span style="float:right;">(검증됨)</span>
- **DaramZi#EUW** <span style="float:right;">(검증 안됨)</span>
- **FTFFMT#EUW** <span style="float:right;">(검증 안됨)</span>
- **Ochris#Korea** <span style="float:right;">(검증 안됨)</span>
- **Pabet#KOR** <span style="float:right;">(검증 안됨)</span>
- **PooParty Edwin#0307** <span style="float:right;">(검증 안됨)</span>
- **Sillim#EUW** <span style="float:right;">(재검증 필요)</span>
- **곽병팔#1649** <span style="float:right;">(검증됨)</span>
    - 개발자의 요구사항을 디코 채널 방장에게 전달함
- **파우더#0517** <span style="float:right;">(재검증 필요)</span>
- **해당아이디는사용할수없습니다#1557** <span style="float:right;">(검증됨)</span>
    - 초기 프로토타입 평가에 기여함