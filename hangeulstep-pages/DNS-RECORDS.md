# 가비아 DNS에 넣을 레코드 (GitHub Pages 연결용)

가비아 로그인 → 서비스 관리 → **DNS 관리툴** → `hangeulstep.com` 선택 → **DNS 설정 > 레코드 수정**

`<GitHub사용자명>` 은 본인 GitHub 계정 이름(예: github.com/abc123 이면 abc123)으로 바꿔 넣는다.

## hangeulstep.com

| 타입 | 호스트 | 값/위치 | 비고 |
|---|---|---|---|
| A | @ | 185.199.108.153 | |
| A | @ | 185.199.109.153 | |
| A | @ | 185.199.110.153 | |
| A | @ | 185.199.111.153 | |
| CNAME | www | `<GitHub사용자명>`.github.io | 저장소 이름은 넣지 않는다 |

- TTL은 기본값으로 둔다.
- 호스트 `@` 칸이 따로 없고 비워 두는 화면이면 비워 둔다.
- `@` 호스트에는 위 A 레코드 4개 외에 다른 A / AAAA 레코드가 없어야 한다. 다른 레코드가 있으면 HTTPS 인증서 발급이 막힐 수 있다.

## (권장) 도메인 소유권 인증 TXT 레코드

다른 사람이 내 도메인을 GitHub Pages에 몰래 연결하는 것을 막는 설정이다. 선택 사항이지만 하는 것을 권한다.

1. GitHub 오른쪽 위 프로필 → **Settings** → 왼쪽 **Pages** → **Add a domain** → `hangeulstep.com` 입력
2. GitHub가 알려 주는 TXT 레코드를 가비아에 그대로 추가한다.

| 타입 | 호스트 | 값 |
|---|---|---|
| TXT | `_github-pages-challenge-<GitHub사용자명>` | GitHub 화면에 표시된 값 |

3. GitHub 화면에서 **Verify** 를 누른다.

## hangeulstep.kr

GitHub Pages 사이트 1개에는 도메인을 1개만 연결할 수 있다. `.kr` 은 `hangeulstep.com` 으로 이동시키는 방식으로 처리한다.

- 가비아 DNS 관리 화면의 **포워딩** 설정에서 `hangeulstep.kr` → `https://hangeulstep.com` 으로 이동하도록 설정한다.
- 우선순위가 낮으므로 `.com` 이 정상 동작한 뒤에 해도 된다.
- 포워딩은 `http://hangeulstep.kr` 접속 기준으로 동작할 수 있고, `https://hangeulstep.kr` 로 직접 접속하는 경우의 동작은 설정 후 직접 확인해야 한다.

## 반영 확인

```bash
nslookup hangeulstep.com
```

결과에 위 4개 IP가 나오면 반영된 것이다. 몇 분에서 몇 시간 걸릴 수 있다.
