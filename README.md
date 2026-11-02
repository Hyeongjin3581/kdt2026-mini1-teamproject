# 담배 리뷰 커뮤니티 웹사이트 (KDT 미니프로젝트 1)

담배 제품 정보 · 리뷰 · 커뮤니티를 제공하는 정적 웹사이트를 백엔드 없이 HTML · CSS · JavaScript와 localStorage로 구현했습니다.

| 항목 | 내용 |
| --- | --- |
| 기간 | 2026.07.28 ~ 07.31 (4일) |
| 팀 | 4명 |
| 내 역할 | **커뮤니티 · 커뮤니티 상세 페이지** 개발, GitHub 저장소 관리 (브랜치 병합) |
| 기술 | HTML · CSS · JavaScript · localStorage · Git / GitHub |

> KDT 첫 팀 프로젝트로, 07.07 ~ 07.27에 배운 HTML · CSS · JavaScript(DOM, localStorage, 쿼리스트링)를 종합했습니다.

## 페이지 구성

| 페이지 | 내용 |
| --- | --- |
| `index.html` | 메인 (최근 등록된 리뷰) |
| `담배브랜드.html` · `연초.html` · `아이코스.html` · `릴.html` | 브랜드 · 제품 목록 |
| `cigar_detail.html` | 제품 상세 + 리뷰 |
| `커뮤니티.html` | 인기글 + 게시글 목록 **(내 담당)** |
| `커뮤상세.html` | 게시글 상세 · 추천 / 비추천 · 댓글 **(내 담당)** |

## 데이터 설계

백엔드가 없어서 JS 배열을 테이블처럼 설계하고, 바뀌는 데이터는 localStorage에 저장했습니다.

| 테이블(배열) | PK | 주요 필드 |
| --- | --- | --- |
| `cigarTable` | cigarID (50000~) | brandID(FK), cigarName, price, nicotine, tar |
| `reviewTable` | reviewID (20000~) | cigarID(FK), memberID(FK), review, score |
| `memberTable` | MemberID (40000~) | userID, userNAME, userPHOTO |
| `postsTable` | postID (60000~) | userID, postTitle, postContent, thumbsUp, thumbsDown |
| `commuList` (댓글) | commuID (30000~) | memberID, content, timeStr |

## 팀원별 담당

| 팀원 | 주요 작업 |
| --- | --- | --- |
| **나** | 커뮤니티 · 커뮤상세 페이지, 저장소 생성 · 브랜치 병합 |
| 팀원 A | 메인(index) · 커뮤니티 데이터 localStorage 연동 |
| 팀원 B | 리뷰 테이블 · 오류 수정 |
| 팀원 C | 제품 상세 · 리뷰 샘플 · 최근 리뷰 출력 수정 |

## 내가 맡은 부분

### 1. 추천 / 비추천 (localStorage 유지)

새로고침하거나 다른 페이지로 갔다 와도 추천 수가 유지되도록 localStorage에 저장하고, 화면의 같은 숫자(상단 / 하단)를 한 번에 갱신했습니다.

```javascript
let currentUp = localStorage.getItem('hotPostUp');
if (currentUp === null) {
    currentUp = 206;
    localStorage.setItem('hotPostUp', currentUp);
} else {
    currentUp = parseInt(currentUp, 10);
}
const updateAllUpCounts = () => {
    currentUp += 1;
    localStorage.setItem('hotPostUp', currentUp);
    renderCounts();
};
```

### 2. 댓글 작성 · 삭제 (본인 댓글만 삭제)

- 댓글 번호는 30000번대에서 가장 큰 번호 + 1로 만들어 중복을 막았습니다.
- 로그인 사용자와 댓글 작성자가 같을 때만 삭제 버튼을 보여주고, 삭제 후 댓글 수 `[N]`도 다시 계산했습니다.

### 3. 커뮤니티 탭 전환

- 전체 / 담배 / 기타 탭을 누르면 해당 목록만 보여주고, 선택한 탭에만 밑줄을 표시했습니다.

### 4. GitHub 저장소 관리

- 저장소를 만들고 팀원별 브랜치(이름)를 나눠 작업했습니다.
- 4일 동안 팀원 브랜치를 main에 병합한 커밋이 45건입니다. 첫 Git 협업을 병합 담당으로 경험했습니다.

| 파일 | 직접 작성 |
| --- | --- |
| `커뮤상세.js` | 290줄 전부 |
| `커뮤니티.js` | 211줄 중 95줄 |

## 실행 방법

1. 저장소를 받은 뒤 VS Code에서 폴더를 엽니다.
2. `index.html`을 Live Server로 엽니다. (`http://127.0.0.1:5500/index.html`)
3. 추천 수 · 댓글은 브라우저 localStorage에 저장되므로, 처음 상태로 돌리려면 개발자 도구(F12) → Application → Local Storage를 비웁니다.
