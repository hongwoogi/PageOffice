# 소스 빌드와 검증 범위

선별한 공개 트리의 클린 빌드는 아직 미검증이다. 원래 로컬 트리에서 검증한 설치본과 새 빌드가 일치한다고 주장하지 않는다. 제품 소스·lock·필수 rhwp 자산은 보존하고 사용자 경로/자료·설치본·캐시·자동 게시 워크플로와 미검토 native 실행 파일은 제외했다.

## 준비

Windows x64, 공식 Node >=22.12/npm >=10/Python 3, edition 2024를 지원하는 Rust/Cargo와 MSVC C++ 빌드 도구가 필요하다. root package-lock.json, native Cargo.lock, isolated Kordoc lock과 UPSTREAM.lock.json을 존중한다. 키 없이도 빌드·오프라인 문서 기능을 사용할 수 있다.

## 절차

1. 루트에서 `npm ci`를 실행한다. 기존 Electron 설치 스크립트는 공식 배포물을 사용한다.
2. `vendor/kordoc-runtime`에서 `npm ci --workspaces=false --ignore-scripts --omit=optional`; 루트로 돌아와 `python tools/prepare-kordoc-runtime.py`를 실행한다. OCR/모델 의존성을 추가하지 않는다.
3. `node tools/build-scrollbar-workspace.mjs --check`로 고정 원본을 대조한다.
4. `npm run build:all`로 편집기·native sidecar·CLI·셸을 빌드한다. native 빌드 실패나 누락을 무시하지 않는다. CLI 버전은 필요 시 `GENOFFICE_APP_VERSION=0.4.0`으로 지정한다.
5. `node tools/gen-third-party-notices.mjs --workspace`로 고지를 준비한다.
6. `npm run dist:workspace -w @genoffice/shell`로 로컬 패키지를 만든다. 공개 게시·서명·Release 업로드는 별도 작업이다.

기존 시험 설치본은 공식 GenOffice 배포의 prebuilt XLSX sidecar를 사용했다. 이 공개 트리에는 미검토 바이너리를 복사하지 않았고 Rust 소스와 Cargo.lock을 포함했다. 새 native 빌드와 기존 배포물의 일치 검증은 아직 미실행이다. rhwp prepared Studio/core는 고정된 MIT 원본 및 폰트/자산 고지와 함께 포함했다. 재생성에는 원본 commit·workspace patch와 고지를 대조해야 한다.

빌드 후 `electron apps/shell`로 실행한다. 별도 프로필·새 vault·복사본을 사용해 생성/저장/재열기/이동/충돌을 확인한다. 일부 과거 tools 검증은 로컬 생성 fixture를 전제로 한다. 개인 테스트 디렉터리를 포함하지 않았으므로 합성 fixture를 재생성해야 하며 실제 키 호출로 대체하면 안 된다. 전체 upstream 테스트가 깨끗하다고 주장하지 않는다.

고지 충돌과 독립 감사 미완료는 [출처 문서](OPEN_SOURCE.md)를 참고한다.
