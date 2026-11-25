# 한글스텝 - GitHub Pages로 무료 시작 (준비 중 페이지)

목표: 서버 없이 무료로 `https://hangeulstep.com` 에 "준비 중" 페이지를 띄운다.

```
hangeulstep-pages/
├── index.html        # 준비 중 페이지 (noindex)
├── 404.html          # 없는 주소로 접속했을 때
├── CNAME             # 연결할 도메인: hangeulstep.com (파일 이름은 반드시 대문자)
├── DNS-RECORDS.md    # 가비아 DNS에 넣을 레코드 목록
└── README.md
```

## 주의

- GitHub 무료 계정에서 GitHub Pages는 **공개(Public) 저장소**에서만 쓸 수 있다. 이 저장소에는 준비 중 페이지만 올린다. **비밀번호, 키, 회원 정보 등 비밀값은 절대 넣지 않는다.**
- `CNAME` 파일에는 도메인 1개만 쓸 수 있다. `hangeulstep.kr` 은 `.com` 으로 이동시키는 방식으로 처리한다(DNS-RECORDS.md 참고).
- 실제 회원·강사 기능 개발은 이 방식으로 할 수 없다. 그 단계에서 가비아 서버로 옮긴다(아래 "서버로 옮길 때").

## 진행 순서

### 1. GitHub 저장소 만들기

1. https://github.com 에 로그인 (계정이 없으면 가입)
2. 오른쪽 위 **+** → **New repository**
3. Repository name: `hangeulstep-site`
4. **Public** 선택, "Add a README file" 등은 체크하지 않는다 → **Create repository**

### 2. 파일 올리기 (쉬운 방법: 웹 업로드)

1. 만든 저장소 화면에서 **uploading an existing file** 링크 클릭
2. 이 폴더의 파일 4개(`index.html`, `404.html`, `CNAME`, `README.md`)를 끌어다 놓는다. `DNS-RECORDS.md` 는 올려도 되고 안 올려도 된다.
3. 아래 **Commit changes** 클릭

(명령어로 하려면)
```bash
cd hangeulstep-pages
git init -b main
git add .
git commit -m "chore: 준비 중 페이지"
git remote add origin https://github.com/<GitHub사용자명>/hangeulstep-site.git
git push -u origin main
```

### 3. GitHub Pages 켜기

1. 저장소 → **Settings** → 왼쪽 **Pages**
2. Build and deployment → Source: **Deploy from a branch**
3. Branch: **main** / **(root)** → **Save**
4. 잠시 뒤 `https://<GitHub사용자명>.github.io/hangeulstep-site/` 에서 준비 중 페이지가 보이면 성공

### 4. 가비아 DNS 설정

`DNS-RECORDS.md` 의 표대로 `hangeulstep.com` 에 A 레코드 4개와 `www` CNAME 을 추가한다.

### 5. 커스텀 도메인 연결

1. 저장소 → **Settings** → **Pages** → **Custom domain** 에 `hangeulstep.com` 입력 → **Save**
2. DNS 확인이 끝나면 **Enforce HTTPS** 를 체크한다. 인증서가 만들어지기 전에는 체크할 수 없으니 잠시 기다린다.
3. 브라우저에서 `https://hangeulstep.com` 접속 확인

### 6. 확인

- `https://hangeulstep.com` 에서 "한글스텝 / 홈페이지를 준비하고 있습니다." 가 보인다.
- `http://hangeulstep.com` 으로 접속해도 HTTPS 로 이동한다.
- `www.hangeulstep.com` 으로 접속해서 같은 사이트가 열리는지 확인한다.

## 문제 해결

| 증상 | 확인할 것 |
|---|---|
| Pages 설정에서 DNS check 실패 | 가비아 A 레코드 4개와 `www` CNAME 이 맞는지, `@` 에 다른 A/AAAA 레코드가 없는지. 반영에 시간이 걸릴 수 있다. |
| Enforce HTTPS 가 비활성 | 인증서 발급 대기 중이다. 잠시 뒤 다시 확인한다. 계속 안 되면 Custom domain 을 지우고 다시 저장한다. |
| 404 | 저장소 루트에 `index.html` 이 있는지, Pages 의 Branch 가 main / (root) 인지 |
| Pages 메뉴가 안 보임 | 저장소가 Public 인지 확인한다. |

## 서버로 옮길 때 (회원·강사 기능 개발 시작 시점)

1. 가비아 클라우드 서버를 만들고 `hangeulstep` 배포 패키지로 환경을 세팅한다.
2. 가비아 DNS 에서 `@` 의 GitHub A 레코드 4개를 지우고, 서버 공인 IP 로 A 레코드를 넣는다. `www` 도 서버 IP 로 바꾼다.
3. GitHub 저장소의 Pages 설정에서 **Custom domain 을 해제**하고, TXT 인증 레코드를 만들었다면 함께 정리한다.
4. 서버에서 HTTPS 인증서를 발급받는다.
