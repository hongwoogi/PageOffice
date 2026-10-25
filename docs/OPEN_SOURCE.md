# 오픈소스 출처와 활용 범위

브랜딩 정리와 원본 고지 삭제를 구분한다. [LICENSE](../LICENSE), [NOTICE](../NOTICE), [고정 원본](../UPSTREAM.lock.json), [수정 기록](../apps/shell/build/PAGEOFFICE-MODIFICATIONS.txt), [고지 전문](../apps/shell/build/THIRD-PARTY-NOTICES.txt)을 유지한다. 법무 검토 완료나 상용 적합성 보증이 아니다.

## 기반 프로젝트

| 구성요소/고정 원본 | 확인한 조건 | 활용·변경 범위 |
| --- | --- | --- |
| [GenOffice](https://github.com/genspark-ai/genoffice), `28cee5ca8a3a1b6b069ea2640493bcb6fce45e0a` | 루트 Apache-2.0/NOTICE, 별도 ee 경계 | Electron/React/TypeScript 셸, Office·Markdown·PDF 엔진과 기존 AI 공급자 경로를 재사용. vault/페이지/가져오기/검색/안전 저장/테마/플로팅 UI 확장. 상용 ee 코드는 제외 |
| [rhwp](https://github.com/edwardkim/rhwp) core/editor `0.8.7`, `1a76570e833917d15817415a53c09ad61ab3203f` | MIT 및 별도 포함 자산/폰트 조건 | Studio/WASM 로컬 통합, HWP/HWPX 저장·제한 명령, host/IPC·workspace patch·손실 보고·원본 보존. [고지](../vendor/licenses) 참조 |
| [Kordoc](https://github.com/chrisryugj/kordoc) `4.24.4`, `1b55975d714900e778798275b6def729db42c68f` | MIT와 배포 NOTICE | 원본 npm archive는 수정하지 않고 Buffer-only 제한 워커 어댑터로 사용. offline/OCR/formula OCR/images off, PDF 페이지별 근거. [원본 lock](../vendor/kordoc-source.lock.json), [원문](../vendor/kordoc-audit-4.24.4/source), [런타임 고지](../vendor/kordoc-runtime-NOTICES.txt) 참조 |

선택적 수식 OCR 모델 가중치는 AGPL 검토 대상이다. 설치본과 공개 소스에 해당 모델 가중치를 포함하지 않았고 다운로드·실행하지 않았다. SDK archive에 선택 기능을 참조하는 코드/문서가 남아 있어도 이를 사용한다고 표시하지 않는다. 모델을 추가하려면 별도 조건을 검토해야 한다.

## 주요 편집기/런타임

정확한 package metadata와 보존한 원문을 기준으로 한 구분이다. 상위 프로젝트 이름만으로 모든 하위 자산의 조건을 대체하지 않는다.

| 패키지/원본 | 버전·패키지 고지 | 활용 |
| --- | --- | --- |
| [Electron](https://github.com/electron/electron) | 43.7.6, MIT | 데스크톱 런타임. Chromium 등의 제3자 조건은 별도이며 Electron 바이너리는 이 트리에 포함하지 않음 |
| [Univer core](https://github.com/dream-num/univer) | 0.25.1, Apache-2.0 | 기존 XLSX 편집기. 하위 OT 충돌은 아래 참조 |
| [Tiptap core](https://github.com/ueberdosis/tiptap) | 3.31.0, MIT | community core 문서/Markdown 편집 구조. 유료 확장 허가를 주장하지 않음 |
| [ProseMirror model](https://github.com/ProseMirror/prosemirror-model) | 1.25.12, MIT | 문서 모델 기반 라이브러리 |
| [PptxGenJS](https://github.com/gitbrent/PptxGenJS) | 4.0.1, MIT | 기존 PPTX 생성/변환 구성요소. 전체 슬라이드 엔진을 이 라이브러리 하나로 설명하지 않음 |
| [EmbedPDF PDFium package](https://github.com/embedpdf/embed-pdf-viewer) | 2.15.1, 패키지 MIT | 기존 PDF 구성. wrapper MIT가 PDFium 및 포함 자산의 개별 조건을 대체하지 않음 |

[정확한 component 원문](../vendor/licenses/components), [전체 고지](../apps/shell/build/THIRD-PARTY-NOTICES.txt), [inventory](../apps/shell/build/WORKSPACE-NPM-INVENTORY.json)를 함께 확인한다. inventory는 보수적인 소스 의존성 범위이며 전부 실제 런타임에 포함된다고 의미하지 않는다. lock의 resolved/integrity도 유지한다.

## 교체·빌드 의존성·미해결 사항

- **react-remove-scroll-bar:** [공식 commit 7301c160fda44cb8cf2b9fdfde61efad35736196](https://github.com/theKashey/react-remove-scroll-bar/commit/7301c160fda44cb8cf2b9fdfde61efad35736196)의 MIT LICENSE·원본 archive·source lock·build provenance를 보존한 workspace 교체. [LICENSE](../packages/react-remove-scroll-bar/LICENSE), [lock](../vendor/react-remove-scroll-bar-source.lock.json), [대조 정보](../packages/react-remove-scroll-bar/BUILD-PROVENANCE.json). 과거 npm 2.3.8의 조건이 소급 해결됐다고 주장하지 않는다.
- **lazy-val 1.0.5:** 비활성 자동 업데이트 경로를 수동 안내 모듈로 대체해 PageOffice 런타임에서는 제거했다. 빌드 도구 의존성에는 남고 고지 문제의 법적 해결을 주장하지 않는다. node_modules 구현을 공개 트리에 복사하지 않았다.
- **ot-text-unicode 4.0.0:** metadata ISC와 정확한 README의 MIT 표기가 충돌한다. 누락 조건을 임의의 표준 전문으로 보완하지 않고 기존 고지를 보존했다. npm 구현은 이 트리에 vendoring하지 않았으나 제품 빌드/Univer 런타임 의존성으로 남으므로 배포 전 해결할 검토 사항이다.
- **native XLSX sidecar:** Rust 소스 Cargo.toml은 Apache-2.0이며 Cargo.lock을 포함한다. 기존 시험 설치본의 prebuilt binary는 새 소스 빌드/공급망 대조를 완료하지 않았으므로 공개 트리에서 제외했다. native 의존성 조건과 새 빌드를 별도 검증해야 한다.

## 폰트·자산·공개 범위

rhwp 폰트는 SIL OFL 등 개별 원문을 유지한다. [폰트 고지](../vendor/licenses/RHWP-FONTS.md)와 각 license를 확인하며 Pretendard/Noto/Nanum/Gowun/Source Han 계열을 하나의 조건으로 단정하지 않는다. 시스템 UI 글꼴 선택은 설치된 글꼴을 읽어 표시할 뿐 글꼴 파일을 새로 배포하거나 OS 설정을 변경하지 않는다.

README 이미지는 실제 0.4.0의 합성 문서 화면이며 조작된 기능 이미지나 가짜 AI 답변이 아니다. 자동 게시 워크플로, 사용자 vault·개인 로그·자격증명·설치 EXE·미검토 native 실행 파일은 제외한다. 독립 법무/보안/네이티브 공급망 감사 및 복잡한 실제 문서 검증은 아직 미완료이다.
