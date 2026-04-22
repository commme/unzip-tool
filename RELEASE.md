# unzip-tool · 릴리즈 메모

> 배포 표준 흐름 전체: [`projects/RELEASE.md`](../../RELEASE.md) 참조.
> 본 문서는 이 앱 고유의 메타데이터만 담는다.

## 앱 메타

| 항목 | 값 |
|------|-----|
| 이름 | unzip-tool |
| 카테고리 | 웹앱 · 파일 변환 |
| GitHub | https://github.com/commme/unzip-tool |
| 라이브 | https://commme.github.io/unzip-tool/ |
| 진입점 | `index.html` |
| 한 줄 설명 | ZIP 파일을 브라우저에서 바로 풀어주는 무료 도구. 드래그 앤 드롭, 한글 파일명 자동 복원, 선택 해제 지원. |
| 기술 스택 | HTML/JS, fflate 0.8.2, Pretendard |
| 태그 | ZIP, 압축해제, 한글파일명, 웹앱 |
| 상태 | 완료 |

## v1 주요 결정

- **지연 해제 아키텍처**: 처음엔 `fflate.unzip()` 한 번에 전체 해제했다가 80MB ZIP에서 30초+ 멈춤. Central Directory만 직접 파싱해서 즉시 목록 표시 + 다운로드 시점에만 `fflate.inflate()` 호출하는 구조로 전환
- **CDN 교체**: cdnjs fflate 404 이슈 → jsDelivr로 교체
- **한글 파일명 로직**: UTF-8 플래그 확인 → fatal UTF-8 디코드 실패 시 EUC-KR 폴백. 플래그 없으면 EUC-KR 우선 시도 (Hangul 범위 감지)
- **파일명 sanitize**: CapCut ZIP처럼 TAB 문자(0x09) 포함 파일명이 들어올 수 있어 OS 금지 문자 `_`로 치환

## 미구현 / 다음 버전

- AES 암호화 ZIP 해제 (fflate 미지원 → 다른 WASM 라이브러리 필요)
- 폴더 구조 유지한 묶음 다운로드 (현재는 선택 파일을 flat ZIP으로)
- 진행률 표시 (큰 파일 해제 시)
- RAR/7Z 지원 (별도 libarchive.js 필요)

## 배포 명령 시퀀스

[`projects/RELEASE.md`](../../RELEASE.md) §10 "한 번에 실행" 참조.
필수 선행: `productivity-qa-reviewer` + `productivity-security-reviewer` 통과.
