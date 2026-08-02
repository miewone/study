# Study Archive

Cloudflare Pages로 배포하는 개인 학습 페이지 모음입니다. 루트의 `index.html`이 `pages.json`을 읽어 주제별 카드와 검색 기능을 제공합니다.

## 디렉터리 구조

```text
.
├── index.html          # 메인 학습 페이지 목록
├── pages.json          # 공개할 페이지의 메타데이터
├── README.md           # 관리 및 배포 안내
└── ontology/
    └── ONTOLOGY_SIX_ELEMENTS_EXPLAINED.html
```

학습 주제가 늘어나면 주제 이름의 디렉터리를 만들고 HTML 문서를 배치합니다. 예를 들어 Cloudflare 관련 글은 `cloudflare/`, Python 관련 글은 `python/` 아래에 둡니다.

## 새 학습 페이지 등록

1. 주제 디렉터리에 HTML 파일을 추가합니다.
2. `pages.json`의 `pages` 배열에 다음 형식으로 항목을 추가합니다.

```json
{
  "title": "페이지 제목",
  "description": "목록 카드에 표시할 짧은 설명",
  "category": "카테고리 이름",
  "tags": ["검색 태그 1", "검색 태그 2"],
  "path": "./directory/page.html",
  "updatedAt": "YYYY-MM-DD"
}
```

- `title`, `description`, `category`, `path`는 필수입니다.
- `tags`와 `updatedAt`은 선택 항목입니다.
- `path`에는 저장소 루트 기준 상대 경로를 사용합니다.
- 목록 순서는 `pages.json`에 작성한 순서를 따릅니다.
- 같은 `category` 값의 페이지는 하나의 섹션으로 묶입니다.

## 로컬에서 확인

`index.html`은 `pages.json`을 `fetch`로 읽으므로 파일을 직접 여는 대신 로컬 HTTP 서버를 사용합니다.

```bash
python3 -m http.server 8000
```

브라우저에서 `http://localhost:8000`을 엽니다.

## Cloudflare Pages 배포

Git 저장소를 Cloudflare Pages 프로젝트에 연결하고 다음과 같이 설정합니다.

| 설정 | 값 |
| --- | --- |
| Framework preset | None |
| Build command | `exit 0` |
| Build output directory | `.` |

정적 파일만 사용하므로 별도의 빌드 도구와 외부 라이브러리가 필요하지 않습니다. Cloudflare는 빌드가 필요 없는 정적 사이트에 `exit 0`을 권장합니다. 기본 브랜치에 변경 사항을 push하면 Cloudflare Pages가 새 버전을 배포합니다.

자세한 연결 절차는 [Cloudflare Pages의 Static HTML 배포 가이드](https://developers.cloudflare.com/pages/framework-guides/deploy-anything/)에서 확인할 수 있습니다.

## 관리 원칙

- 각 문서는 필요한 CSS와 JavaScript를 자체 포함하여 독립적으로 열 수 있게 유지합니다.
- 파일명은 URL 호환성을 위해 영문, 숫자, 하이픈 또는 밑줄을 사용합니다.
- 파일을 이동하거나 이름을 바꿀 때는 `pages.json`의 `path`도 함께 수정합니다.
- 배포 전 모든 `path`가 실제 파일을 가리키는지 확인합니다.
