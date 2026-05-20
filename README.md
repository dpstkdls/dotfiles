# dotfiles

여러 macOS PC에서 동일한 개발 환경을 유지하기 위한 설정 저장소.

## 구성

| 파일 | 내용 |
|------|------|
| `Brewfile` | 모든 PC에 공통으로 설치할 Homebrew 패키지·앱·VS Code 확장 |

## 새 PC 셋업

### 1. Homebrew 설치

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

### 2. 저장소 클론

```bash
git clone <repo url> ~/dotfiles
```

### 3. 공통 패키지 설치

```bash
cd ~/dotfiles
brew bundle install --file=Brewfile
```

## 일상 운영

### 변경 사항 반영

새로 설치한 패키지 중 **공통으로 관리할 것**만 골라 `Brewfile`에 직접 추가하고 커밋합니다.

```bash
# 예시: 새 패키지 추가
echo 'brew "ripgrep"' >> ~/dotfiles/Brewfile
cd ~/dotfiles && git add Brewfile && git commit -m "Add ripgrep"
```

### 현재 상태와 Brewfile 비교

이 PC에 설치돼 있지만 `Brewfile`에는 없는 항목을 확인:

```bash
brew bundle cleanup --file=~/dotfiles/Brewfile
```

> `--force` 옵션을 붙이면 실제로 제거합니다. 평소에는 확인용으로만 쓰는 게 안전합니다.

### 전체 dump 후 수동 정리 (정기 점검용)

월 1회 정도, 모든 설치 항목을 dump해서 누락된 공통 항목이 있는지 점검:

```bash
brew bundle dump --describe --force --file=/tmp/Brewfile.snapshot
diff ~/dotfiles/Brewfile /tmp/Brewfile.snapshot
```

## 운영 원칙

- **Brewfile-first**: 오래 쓸 도구는 Brewfile에 먼저 추가하고 `brew bundle install`로 깐다.
- **일회성 실험**은 그냥 `brew install`로 깔고 끝나면 `brew uninstall`.
- **의존성 라이브러리**(cairo, jpeg, pango 등)는 Brewfile에 직접 추가하지 않는다.
- **회사·프로젝트 전용 패키지**는 별도 Brewfile로 분리해서 PC마다 선택적으로 적용한다.
