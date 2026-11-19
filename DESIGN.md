# 오늘의 타로 디자인 가이드

## 1. 시각 방향

밝은 갤러리처럼 여백이 넓은 화면 가운데 타로 카드 한 장을 둔다. 흰색과 옅은 보라색으로 화면을 구성하고, 진한 자주색은 주요 행동과 선택 상태에 사용한다. 금색과 짙은 보라색은 카드 삽화에 사용한다. 그림자 없이 얇은 경계선과 배경색으로 영역을 구분한다.

시작 → 내 정보와 주제 → 맞춤 해석의 세 화면으로 구성한다. 모바일 우선 라이트 테마이며 카드 그림, 한국어 제목, 다음 행동 순으로 시선이 흐르게 한다. 서비스 이름은 **오늘의 타로**로 통일한다.

## 2. 색상과 Tailwind CSS v4

전역 CSS에서 의미 기반 변수를 선언하고 `@theme inline`으로 Tailwind 유틸리티에 연결한다. shadcn 컴포넌트는 `bg-background`, `text-muted-foreground`, `bg-primary`처럼 역할이 드러나는 클래스를 사용한다.

```css
@import "tailwindcss";

:root {
  --background: #ffffff;
  --foreground: #211a30;
  --card: #ffffff;
  --card-foreground: #211a30;
  --muted: #f6f3fa;
  --muted-foreground: #655b73;
  --primary: #603c8d;
  --primary-foreground: #ffffff;
  --primary-hover: #503176;
  --primary-active: #41265f;
  --secondary: #ede6f6;
  --secondary-foreground: #392354;
  --accent: #efe7fa;
  --accent-foreground: #4b2b70;
  --border: #ded5e8;
  --input: #8b7b9b;
  --ring: #6b429a;
  --destructive: #a22542;
  --success: #246247;
  --disabled: #ece8f1;
  --disabled-foreground: #655b73;
  --art-ink: #291c40;
  --art-gold: #d0ad69;
  --radius: 1rem;
  --taro-font: "Pretendard", "Noto Sans KR", "Malgun Gothic", system-ui, sans-serif;
}

@theme inline {
  --color-background: var(--background);
  --color-foreground: var(--foreground);
  --color-card: var(--card);
  --color-card-foreground: var(--card-foreground);
  --color-muted: var(--muted);
  --color-muted-foreground: var(--muted-foreground);
  --color-primary: var(--primary);
  --color-primary-foreground: var(--primary-foreground);
  --color-primary-hover: var(--primary-hover);
  --color-primary-active: var(--primary-active);
  --color-secondary: var(--secondary);
  --color-secondary-foreground: var(--secondary-foreground);
  --color-accent: var(--accent);
  --color-accent-foreground: var(--accent-foreground);
  --color-border: var(--border);
  --color-input: var(--input);
  --color-ring: var(--ring);
  --color-destructive: var(--destructive);
  --color-success: var(--success);
  --color-disabled: var(--disabled);
  --color-disabled-foreground: var(--disabled-foreground);
  --color-art-ink: var(--art-ink);
  --color-art-gold: var(--art-gold);
  --font-sans: var(--taro-font);
  --radius-control: calc(var(--radius) - 4px);
  --radius-panel: calc(var(--radius) + 8px);
  --radius-art: var(--radius);
}
```

| 역할 | 사용 위치 |
|---|---|
| `background / foreground` | 화면 바탕, 제목, 본문 |
| `muted / muted-foreground` | 카드 배경 영역, 개인정보 안내와 보조 문구 |
| `primary / primary-foreground` | 주요 버튼, 텍스트 링크 |
| `secondary / secondary-foreground` | 보조 버튼 |
| `accent / accent-foreground` | 선택된 주제, 고스트 버튼 호버 |
| `border / input / ring` | 영역 구분선 / 입력 및 선택 경계 / 키보드 포커스 |
| `destructive / success` | 오류와 성공 안내. 항상 문구를 함께 표시 |
| `art-ink / art-gold` | 카드 일러스트와 장식 |

일반 텍스트 4.5:1, 큰 텍스트와 입력 경계·포커스 등 의미 있는 비텍스트 요소는 3:1 이상을 검수한다. 주요 버튼의 흰색 글자와 보라색 배경은 8.3:1, 보조 문구와 옅은 보라색 배경은 5.8:1, 입력 경계와 흰색 배경은 3.9:1이다. 포커스는 2px 링에 2px 오프셋을 적용한다.

## 3. 글씨, 간격, 형태

| 역할 | 크기 / 줄 높이 | 굵기 | 자간 |
|---|---|---:|---|
| 시작 제목 | 모바일 36/44px, 넓은 화면 48/56px | 700 | -0.035em |
| 화면 제목 | 28/36px | 700 | -0.03em |
| 카드 이름 | 22/30px | 700 | -0.02em |
| 본문·해석·입력 | 16/25px | 400 | -0.015em |
| 버튼·필드 라벨 | 15/22px | 600 | -0.01em |
| 도움말·상태·오락용 문구 | 14/21px | 400 | 기본 |

폰트 파일을 쓰는 경우 앱에 포함하고, 로드에 실패하면 시스템 한글 글꼴을 사용한다. 해석은 문장 단위로 줄을 나누고 한국어 단어 경계를 우선한다. 좁은 화면과 큰 글씨에서는 자연스럽게 감아 읽을 수 있게 하며 말줄임을 적용하지 않는다.

- 간격은 4px 단위로 잡는다. 라벨과 입력 8px, 같은 행의 버튼 8px, 필드 사이 16px, 영역 사이 24px, 화면 주요 구획 사이 32~48px.
- 좌우 여백은 모바일 20px, 360px 미만 16px, 큰 화면 32px. 중앙 열은 폼 440px, 결과 480px, 시작 560px를 상한으로 둔다.
- 버튼과 입력은 최소 높이 48px, 아이콘 단독 버튼은 44×44px. 일반 조작 요소는 12px, 화면의 결과 카드와 안내 패널은 24px, 삽화는 16px 모서리로 통일한다.
- 장식은 카드 그림 안에 둔다. 콘텐츠는 320px 너비부터 가로 스크롤 없이 읽을 수 있어야 한다. 확대·큰 글씨에서 주제나 버튼이 좁아지면 열 수를 줄인다.

## 4. shadcn/ui 컴포넌트 규칙

| 컴포넌트 | 규칙 |
|---|---|
| `Button` | `default`는 타로 시작·카드 한 장 뽑기·이미지 저장. 기본 → `primary-hover` → `primary-active` 상태를 둔다. `outline`은 결과 공유, `secondary`는 다시 뽑기, `ghost`는 뒤로·처음으로. 비활성은 `disabled` 토큰과 실제 `disabled` 속성 사용. 처리 중에는 필요한 버튼을 비활성화하고 `Spinner`와 현재 동작을 표시한다. |
| `FieldGroup` / `Field` / `Input` | 보이는 라벨을 입력과 연결한다. 이름은 닉네임 허용, 최대 10자. 생년월일은 실제 날짜와 미래 여부를 확인한다. 전공 라벨에 ‘선택’을 표시한다. 오류는 `Field`에 `data-invalid`, 입력에 `aria-invalid`, 오류 설명에 `aria-describedby`를 연결한다. 검증 문구는 “날짜를 다시 확인해 주세요”. |
| `ToggleGroup` / `ToggleGroupItem` | 커리어·재물·연애·건강·학업·종합을 단일 선택으로 구성한다. 기본 3열, 공간이 부족하면 2열. 선택 시 보라색 틴트, 진한 테두리, 체크 표시를 함께 적용한다. 그룹에 접근 가능한 이름을 제공하고 설치된 프리미티브의 키보드 동작을 유지한다. |
| `Card` | `CardHeader`에는 이름·주제, `CardContent`에는 그림·카드 이름·별자리·해석, `CardFooter`에는 오락용 안내와 서비스 이름을 둔다. 흰색, 1px 경계, 24px 모서리와 20~24px 패딩. 저장·공유 조작과 개인정보 안내는 카드 바깥에 둔다. |
| `Alert` | 개인정보 안내와 복구 안내에 사용한다. 잠금 아이콘과 문구를 함께 보여 준다. 정상 상태 안내는 `role="status"` 또는 동등한 정중한 라이브 영역으로 알린다. |
| `Dialog` | 이미지 미리보기와 수동 복사용 메시지에 사용한다. 제목과 닫기 버튼을 제공하고, 닫으면 실행한 버튼으로 포커스를 복귀시킨다. 이미지는 길게 누를 수 있게 실제 이미지 요소로 표시하고 텍스트는 선택 가능하게 둔다. |

색상·글씨·높이·모서리는 공통 토큰과 컴포넌트 variant에서 관리하고 화면의 `className`은 폭·그리드·`gap-*` 등 배치에 사용한다. 버튼 아이콘은 `data-icon="inline-start"`로 배치한다. 단독 아이콘 버튼에는 접근 가능한 이름을 지정한다. 앱을 구현할 때 실제 `components.json`의 Base UI/Radix, 아이콘, 경로 설정을 확인해 해당 API를 사용한다.

## 5. 화면별 배치와 상태

### S-01 시작 · F-1

가운데 정렬로 서비스 이름 → 카드 뒷면 → “내 정보를 넣고 카드 한 장으로 나만의 해석을 받아 봐요” → [타로 시작] → “오락용 타로 · 정보는 저장 안 해요”를 배치한다. 카드 뒷면은 약 2:3으로 표시하고, 작은 화면에서도 시작 버튼이 가까이 보이도록 그림 높이를 조절한다.

### S-02 내 정보 · 주제 · F-2

뒤로 → “나에 대해 알려 주세요” → 이름·생년월일·전공 → 개인정보 안내 → “어떤 주제가 궁금한가요?” → 주제 6개 → [카드 한 장 뽑기] 순서다. 실명 대신 닉네임을 권장한다. 이름·실제 생년월일·주제가 유효할 때 버튼을 활성화한다. 키보드가 열린 상태에서도 폼과 버튼을 스크롤해 접근할 수 있게 한다.

### S-03 맞춤 해석 결과 · F-3~F-7

메이저 아르카나 22장 중 뽑힌 정방향 카드 한 장을 표시한다. 뒷면에서 앞면으로 480ms 동안 한 번 뒤집고 1초 이내에 완료한다. 모션 감소 설정에서는 즉시 결과를 표시한다. 결과를 열면 결과 제목으로 초점을 옮긴다.

카드에는 이름·주제, 그림, 카드 번호와 이름, 별자리, 해석, “※ 오락용 타로 해석입니다”를 표시한다. 해석은 3문장·90자 이내로 구성한다. 카드 × 주제 기본 문장과 별자리 문장을 표시하고, 전공 계열이 확인되면 세 번째 문장을 추가한다. 건강·재물 문구는 돌아볼 관점으로 쓴다. 삽화는 직접 제작하거나 이용 허가가 명확한 작품을 사용한다.

카드 아래에는 “저장·공유 결과에 이름이 들어가요”를 안내하고 [이미지 저장] [결과 공유]를 나란히 둔다. 다음 줄은 [다시 뽑기] [처음으로]다. 다시 뽑기는 개인 입력을 유지한 채 주제 선택으로 돌아가고, 처음으로는 입력과 결과를 비우고 S-01로 이동한다.

입력과 결과 화면에 아래 안내를 동일하게 표시한다.

> 입력한 이름 · 생년월일 · 전공은 해석을 만드는 데만 쓰고, 서버로 보내거나 저장하지 않아요. 창을 닫거나 [처음으로]를 누르면 바로 지워져요.

입력값·주제·결과는 화면 메모리에서만 관리한다. 새로 고침·창 닫기·처음으로에서 초기화하며 서버, 브라우저 저장소, 분석 도구로 전송하거나 기록하지 않는다.

## 6. 카드 내보내기와 피드백

- **PNG 1080×1350** · 결과 영역의 시각 순서를 유지한다. 바깥 여백 72px, 카드 그림 중앙 정렬, 하단에 오락용 안내와 서비스 이름. 최대 길이 이름과 해석도 잘리지 않게 확인한다. 생년월일과 전공 원문은 포함하지 않는다.
- **공유** · 지원되는 기기에서는 PNG와 글을 함께 전달한다. 글은 이름·주제 → 카드 이름·별자리 → 해석 → 오락용 안내 → “나도 한 장 뽑기 → {시작 링크}” 순서다. 링크는 개인 값 없는 시작 화면 주소를 사용한다.
- **저장 성공** · “이미지를 저장했어요”. 브라우저가 내려받기 요청까지만 알 수 있는 경우 “이미지 다운로드를 시작했어요”를 표시한다.
- **저장 미지원 또는 실패** · 이미지를 띄우고 “이미지를 길게 눌러 저장해 주세요”.
- **공유 메뉴 열림·취소** · 결과 화면으로 돌아온다. 취소 시 자동 복사하지 않는다.
- **공유 미지원** · “결과를 복사했어요. 원하는 앱에 붙여 넣어 주세요”. 클립보드 쓰기마저 실패하면 메시지를 표시하고 “아래 글을 길게 눌러 복사해 주세요”.

## 7. 검수 기준

- 320px·375px 모바일과 넓은 화면에서 화면 흐름, 버튼 크기, 글자 확대, 줄바꿈을 확인한다.
- 키보드만으로 주제 선택, 날짜 오류 확인, 결과 이동, 저장·복사 대체 경로를 이용할 수 있어야 한다.
- 필수값 누락, 잘못된 날짜, 10자 이름, 전공 생략·미분류, 다시 뽑기와 초기화, 모션 감소를 확인한다.
- 내보낸 이미지와 메시지에서 생년월일·전공 원문이 없고, 링크가 시작 화면을 열며, 결과 화면·이미지·메시지에 오락용 안내가 표시되어야 한다.
- 카드 삽화 출처와 개인정보 동의 체크 필요 여부는 기획 검토 항목이다.

## 기획 근거

- [PRD](TARO_01_PRD.md)
- [기능명세서](TARO_02_기능명세서.md)
- [유저플로우](TARO_03_유저플로우.md)
- [와이어프레임](TARO_04_와이어프레임.md)
