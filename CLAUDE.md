# lesson3_7_student_web — 학생용 배포 폴더

## 폴더 목적
고등학교 정보 교과 학습 웹앱의 **학생 배포본**이자 **전체 학습 포털**.
GitHub Pages 에 이 폴더를 그대로 올리는 것을 전제로 만들었다.

현재 「다양한 제어 구조」(5차시)와 「객체를 구현하는 클래스」(4차시) 두 단원이
들어 있고, **앞으로 다른 단원도 이 루트 `index.html` 에 이어 붙인다.** 그래서
머리말은 특정 단원에 매이지 않는 문구(「배우고, 탐구하고, 해결하는 정보 학습」)로
두고, 단원별 설명은 각 단원 블록의 `.unit-head` 안에 쓴다.
단원을 추가할 때 머리말과 안내 3줄은 고치지 않는다.

**교사용 파일은 이 폴더에 절대 두지 않는다.** 교사용 결과 분석 페이지
(`lesson3_7_*_teacher.html`)는 각 원본 프로젝트의 `dist/` 에만 있고,
선생님이 따로 보관한다. 학생용 사이트에서는 교사용 페이지가 있다는 사실 자체가
드러나지 않아야 한다 — 루트 `index.html` 에 교사용 링크·메뉴·버튼을 넣지 않는다.

## 폴더 구조 — 「단원 → 차시」 두 단계 (2026-08-16 통일)
```
lesson3_7_student_web/
├─ index.html            ← 학생용 학습 포털. 단원 목록. 사람이 직접 편집한다
├─ .nojekyll             ← GitHub Pages 의 Jekyll 처리를 끈다 (아래 설명)
├─ .gitignore            ← CLAUDE.md 등 개발용 파일을 GitHub 에 올리지 않도록 제외
├─ lesson3_7/            ← 단원: 다양한 제어 구조
│   ├─ lesson1/index.html   ← lesson3_7_1/dist/index.html 복사본
│   ├─ lesson2/index.html   ← lesson3_7_2/dist/index.html
│   ├─ lesson3/index.html   ← lesson3_7_3/dist/index.html
│   ├─ lesson4/index.html   ← lesson3_7_4/dist/index.html
│   └─ lesson5/index.html   ← lesson3_7_5/dist/index.html
└─ lesson3_8/            ← 단원: 객체를 구현하는 클래스
    ├─ lesson1/index.html   ← lesson3_8_1/dist/index.html
    ├─ lesson2/index.html   ← lesson3_8_2/dist/index.html
    ├─ lesson3/index.html   ← lesson3_8_3/dist/index.html
    └─ lesson4/index.html   ← lesson3_8_4/dist/index.html
```

**이름 규칙은 하나뿐이다.**

| 원본 프로젝트 | 배포 위치 |
| --- | --- |
| `lesson3_7_N` | `lesson3_7/lessonN/index.html` |
| `lesson3_8_N` | `lesson3_8/lessonN/index.html` |
| `lesson3_9_N` | `lesson3_9/lessonN/index.html` |
| `lesson4_1_N` | `lesson4_1/lessonN/index.html` |

즉 **원본 이름의 마지막 `_N` 을 떼면 단원 폴더, 그 `N` 이 차시 폴더**다.
단원이 늘어나도 이 규칙만 지키면 되고, 배치 파일은 단원 목록 한 줄만 고치면 된다.

> 2026-08-16 이전에는 차시가 루트 바로 아래(`lesson1` ~ `lesson5`)에 있었다.
> 지금은 단원 폴더 아래로 옮겨졌고, 옛 구조 폴더가 남아 있으면
> `build-student.bat` 이 실행할 때 자동으로 지운다.

각 `index.html` 은 **완전한 단일 파일**이다. CSS·JS 가 모두 HTML 안에
인라인되어 있고 외부 라이브러리·폰트·CDN·이미지 파일을 하나도 쓰지 않는다.
그래서 같은 폴더에 딸린 파일이 없고, 페이지를 열 때 요청이 HTML 한 개로 끝난다.
(확인: 브라우저 네트워크 탭에 HTML 1건만 보이면 자기완결형이다.)

## 갱신 방법
원본 프로젝트에서 `npm run build` 를 돌린 뒤, 프로젝트 루트에서

```bash
build-student.bat
```

- **단원 폴더 아래 차시 폴더만** 새로 채운다.
- 루트 `index.html` 은 건드리지 않는다. 포털 내용을 바꾸려면 그 파일을 직접 고친다.
- 교사용 HTML 은 어떤 경우에도 복사하지 않고, 마지막에 폴더 전체를 훑어
  이름에 `teacher` 가 든 파일과 **내용에 교사용 표시(`teacher-main`·`t-main`)가
  있는 HTML** 이 없는지 검사한다.
- 옛 구조 폴더(루트 바로 아래 `lesson1` 같은, 밑줄 없는 `lesson`+숫자)를 지운다.
- 어느 차시에서 실패했는지 `[오류]` 로 표시하고 종료 코드 1 을 낸다.

### 배포할 단원 목록 — 배치 파일에서 고치는 곳은 여기 한 줄뿐
`build-student.bat` 위쪽에 있다.
```bat
set "UNITS=3_7:5 3_8:4"
```
`단원번호:마지막차시` 를 빈칸으로 구분해 적는다.
`3_8:4` 는 「`lesson3_8_1` ~ `lesson3_8_4` 를 `lesson3_8\lesson1` ~ `lesson4` 로」 라는 뜻이다.

| 하고 싶은 일 | 고치는 방법 |
| --- | --- |
| 3_8 단원에 5차시 추가 | `3_8:4` → `3_8:5` |
| 3_9 단원(3차시) 추가 | 뒤에 `3_9:3` 을 덧붙인다 |
| 4_1 단원(6차시) 추가 | 뒤에 `4_1:6` 을 덧붙인다 |

`build-teacher.bat` 에도 같은 줄이 있다. **두 파일의 값을 같게 맞춰 둔다.**
루트 `index.html` 의 단원 블록·링크는 손으로 추가해야 한다(아래 「단원을 추가할 때」).

차시 폴더 안은 복사 전에 비우므로, 여기에 손으로 만든 파일을 두면 사라진다.
직접 관리할 파일은 루트에 둘 것.

## GitHub Pages 배포 (2026-08-12 자동화)
| 주소 | |
| --- | --- |
| 저장소 | https://github.com/happycomhappy/informatics-learning |
| 사이트 | https://happycomhappy.github.io/informatics-learning/ |

### 평소 작업 — 배치 파일 하나
```bat
deploy-student.bat
```
루트에서 실행하면 `build-student.bat` 호출 → 구조 검사 → 교사용 유출 검사 →
`git add` → `commit` → `push origin main` 까지 한 번에 끝난다.
GitHub 웹에서 「Add file → Upload files」로 올릴 필요가 없다.

**멈추는 조건** (이때는 GitHub 에 아무것도 올라가지 않는다)
- 빌드 실패
- `index.html` · `.nojekyll` 이 없음
- 단원 폴더가 하나도 없거나, 단원 폴더에 차시 폴더가 없거나,
  차시 폴더에 `index.html` 이 없음
- 옛 구조 폴더(밑줄 없는 `lesson`+숫자)가 루트에 남아 있음
- 이름에 `teacher` 가 든 파일, 또는 **내용에 교사용 표시(`teacher-main`·`t-main`)가
  있는 HTML** (교사용 파일을 `index.html` 로 이름만 바꿔 놓아도 잡힌다)
- 현재 브랜치가 `main` 이 아님
- push 거부 — 원인을 인증/원격 앞섬/네트워크/브랜치로 나눠 안내한다

**단원·차시 폴더는 `lesson*` 을 두 단계로 훑어서 자동으로 찾는다.**
단원이 늘든 차시가 늘든 `deploy-student.bat` 은 고칠 필요가 없다.
(고치는 곳은 `build-student.bat` 의 `UNITS` 한 줄뿐이다.)

### 처음 한 번만 — Git 연결
```bat
setup-student-git.bat
```
원격 저장소를 임시 폴더에 clone 한 뒤 **`.git` 폴더만 옮겨 온다.**
로컬 HTML 은 하나도 덮어쓰지 않고 GitHub 의 기존 commit 기록도 그대로 남는다.
`git init` · `push --force` · `reset --hard` 는 쓰지 않는다.

이 스크립트는 `core.autocrlf` 를 `false` 로 둔다. 켜져 있으면 줄바꿈만 바뀐
**가짜 변경**이 매번 생겨 기록이 지저분해지기 때문이다.

### 인증
아이디·비밀번호·토큰을 배치 파일에 적지 않는다. Windows 의
**Git Credential Manager** 가 처리하므로 처음 push 할 때 뜨는 로그인 창에서
한 번만 로그인하면 이후에는 자동이다.

## 루트 index.html 의 구조
```
hero            머리말. 단원과 무관한 포털 문구. 단원을 추가해도 고치지 않는다
guide           학습 안내 3줄. 역시 단원과 무관하게 쓴다
main
 └ .unit       ← 단원 하나 = 이 블록 하나. 단원마다 하나씩 늘어난다
     .unit-head   단원 제목 + 단원 설명(차시 수, 단계 수 같은 단원별 정보)
     .steps       차시 카드 행 (<ol> · <li class="step">)
footer
```

### 차시 카드의 디자인 원리
가로로 흐르는 5단 화살표 도식이다. 카드 하나가 다음 세 겹으로 되어 있다.

| 요소 | 정체 | 만드는 곳 |
| --- | --- | --- |
| 왼쪽에 삐져나온 색 탭 | 카드 뒤에 겹쳐 놓은 색 판 | `.step::before` |
| 흰 카드 | 실제 내용 | `.step-card` |
| 오른쪽 화살표 | 다음 차시로 이어지는 표시 | `.step::after` (`clip-path` 삼각형) |

**색은 `<li class="step" style="--c: …; --c-ink: …">` 두 변수로만 정한다.**
탭·화살표·아래쪽 버튼·글머리 점이 `--c` 를 함께 쓰고, 작은 글자는 진한
`--c-ink` 를 쓴다. 카드 안 CSS 를 건드릴 필요가 없다.

현재 색 순서(참고 이미지와 같다): 앰버 `#E9A83A` → 초록 `#5FA85C` →
하늘 `#66B4CE` → 파랑 `#4272B0` → 브라운 `#7B6C61`.

아래쪽 색 띠가 그대로 「학습 시작」 버튼(`.step-go`)이다. `.step-go::after` 가
카드 전체를 덮고 있어서 **카드 아무 곳이나 눌러도 열린다.**

### 단원을 추가할 때
1. `build-student.bat` 과 `build-teacher.bat` 의 `UNITS` 줄에 `3_9:3` 처럼 덧붙인다.
2. `<section class="unit">` 블록을 통째로 복사해 `main` 안에 붙인다.
3. `.unit-head` 의 제목과 설명을 바꾼다.
4. 차시 수만큼 `<li class="step">` 을 남기고 번호·제목·핵심 내용·링크를 바꾼다.
5. 링크는 반드시 `./단원폴더/차시폴더/` 상대경로로 쓴다(아래 「경로 규칙」).
   예: `./lesson3_9/lesson1/`
6. `<ol class="steps" style="--n:3">` 의 `--n` 을 **그 단원의 차시 수**로 맞춘다.

**차시가 5개가 아니어도 된다.** 한 줄에 놓을 카드 수는 CSS 를 고치지 않고
`<ol class="steps" style="--n:4">` 의 `--n` 하나로 정한다(적지 않으면 5).
다만 한 줄에 6개 이상은 카드가 너무 좁아지므로, 그때는 화살표 흐름을 포기하고
`.steps` 를 `repeat(auto-fit,minmax(220px,1fr))` 로 두고
`.step::after{display:none}` 를 주는 편이 낫다.

### 화살표가 1180px 이하에서 사라지는 이유
화살표는 「왼쪽에서 오른쪽으로 이어진다」는 뜻이다. 카드가 여러 줄로 접히면
줄 끝의 화살표가 벽을 향하게 되어 흐름을 잘못 읽게 만든다. 그래서 한 줄에 5개가
들어가지 않는 폭에서는 화살표를 감춘다. `.steps` 의 `padding-right:30px` 도
같은 이유로, 05 뒤 화살표가 화면 밖으로 삐져나와 가로 스크롤이 생기는 것을 막는다.

## 왜 `.nojekyll` 이 필요한가
GitHub Pages 는 기본적으로 Jekyll 로 파일을 한 번 처리한다. Jekyll 은 `{{ ... }}` 를
템플릿 문법으로 해석하는데, 4·5차시 학습 웹앱의 자바스크립트 안에 `{{` 가 들어 있다.
`.nojekyll` 이 없으면 이 부분이 망가지거나 배포가 실패한다. **지우지 말 것.**

## 경로 규칙
링크는 전부 상대경로(`./lesson3_7/lesson1/`)다. GitHub Pages 는 저장소 이름이 붙은
하위 경로(`https://<계정>.github.io/<저장소>/`)로 서비스되므로,
`/lesson3_7/lesson1/` 같은 루트 절대경로를 쓰면 깨진다.
**새 링크를 넣을 때도 반드시 `./단원폴더/차시폴더/` 형태로 쓸 것.**

각 차시 페이지는 자기완결형 단일 HTML 이라 폴더가 한 단계 깊어져도
안에서 바깥 파일을 부르는 곳이 없다. 그래서 이번 구조 변경으로 깨진 경로는 없다.
앞으로 빌드가 `assets/` 같은 폴더를 만들게 되면 그 폴더도 차시 폴더 안으로
함께 복사되므로(배치 파일이 처리한다) 그때도 상대경로가 유지된다.

## 미리보기
`.claude/launch.json` 의 `lesson3_7_student_web` (http://localhost:8091).
GitHub Pages 와 같은 구조로 띄우므로, 여기서 정상 동작하면 배포 후에도 동작한다.

## 원본 프로젝트를 고치지 않는다
이 폴더는 **복사본 전용**이다. 내용을 고쳐야 하면 `lesson3_7_1` ~ `lesson3_7_5` 의
`src/` 를 고치고 다시 빌드한 다음 `build-student.bat` 을 실행한다.
여기서 직접 고친 `lessonN/index.html` 은 다음 갱신에서 사라진다.
