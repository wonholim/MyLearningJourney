# CLI, GUI 버전 관리 환경 구축

- [x] CLI 버전 관리 환경 구축
  - [x] git 구축
  - [x] gh 구축
- [ ] GUI 버전 관리 환경 구축
  - [x] sourcetree 구축
  - [x] gitkraken 구축
---

<br/>

## CLI 버전 관리 환경 구축

---

<br/>

### git 구축

<br/>

- `git은 분산 버전 관리 시스템 중 하나로, 소프트웨어를 개발할 때, 소스 코드의 변경 이력을 추적하고 여러 사람이 동시에 같은 프로젝트를 작업할 떄 변경 사항을 병합하고 관리하기 위해 사용된다.`

<br/>

1. [gitDownload](https://git-scm.com/downloads)에 들어가서 자신에게 맞는 환경을 선택해 다운 받는다.
![image](https://user-images.githubusercontent.com/56383948/262590740-019ec124-459c-410c-a00f-58c34700ca31.png)

<br/>

2. git --version을 입력해서, 설치가 되었는지 확인한다.
![image](https://user-images.githubusercontent.com/56383948/262591130-dcdfffcd-f4af-44b5-aef8-02b1f51b8007.png)

<br/>

3. git config 파일에서, user.name과 user.email을 등록한다.
    ```text
    git config --global user.name "Youname"
    git config --global user.email "youmail@example.com"
    ```
    - 설정을 확인하려면 `git config --list` 명령어를 입력한다.

![image](https://user-images.githubusercontent.com/56383948/262591643-45bf9ca0-941c-4e12-89ad-9760b96f86cc.png)

<br/>

4. github로 돌아가서, repo를 선택해서, HTTPS clone을 한다.
![image](https://user-images.githubusercontent.com/56383948/262591972-d5cb50f6-90a5-4da5-a473-886e9fd53bd5.png)


<br/>

5. `git clone URL` 명령을 통해, 원격 저장소의 파일을 로컬로 다운 받을 수 있다.

<br/>

6. SSH Key를 이용하려면, GitHub 개인 계정에서 새로운 SSH Key를 생성해야한다. (만약 기존에 키를 가지고 있는지 모르겠다면, `ls -al ~/.ssh` 명령을 입력해서 .pub로 끝나는 것이 있는지 확인한다.) 
![image](https://user-images.githubusercontent.com/56383948/262593693-893cf13c-f1af-4523-8468-e6ff962721fc.png)

<br/>

7. 터미널에 들어가서, `cd ~/.shh`를 입력해서, 들어간 뒤 `ssh-keygen -t ed25519 -C "your_email@example.com"` 명령어를 입력한다.
![image](https://user-images.githubusercontent.com/56383948/262595307-3efe1a15-da7d-46d1-a76b-8f41758576e2.png)

<br/>

8. `cat 만든키.pub`를 입력해서, 안의 내용을 복사한다.

<br/>

9. GitHub의 add new SSH Keys에 key에 복사한 내용을 붙여넣는다.
![image](https://user-images.githubusercontent.com/56383948/262592475-2048cd50-4cfb-48bf-a2e9-af0bea423bd8.png)
![image](https://user-images.githubusercontent.com/56383948/262596815-ed835363-93e8-4e68-b0da-a9533ce56e0f.png)

<br/>

10. 생성이 잘 되었는지 확인한다.
![image](https://user-images.githubusercontent.com/56383948/262597409-574c8e10-c7f6-413d-87b0-20ade1da9466.png)

<br/><br/>

### gh 구축

<br/>

`gh는 GitHub CLI를 말하며, 터미널 환경 또는 명령 프롬프트에서 사용이 가능하게 해주는 GitHub 공식 도구이다. GitHub 웹페이지에서 하던 이슈, PR, 저장소 생성 등 기타 GitHub관련 작업을 수행할 수 있다.`

<br/>

1. [gh](https://cli.github.com/)에 들어가서 자신에게 맞게 설치한다.
![image](https://user-images.githubusercontent.com/56383948/262599554-bff2ea3a-09dd-41c3-b161-9bbe4735b8b1.png)
![image](https://user-images.githubusercontent.com/56383948/262600250-b92a0d50-64f6-4cb9-b341-0be32efec361.png)

<br/>

2. `gh version` 명령어를 입력해서, 설치가 잘 되었는지 확인한다.
![image](https://user-images.githubusercontent.com/56383948/262600532-dfd2c0ef-e12e-486f-bca8-f7531feeef86.png)

<br/>

3. GitHub 계정으로 인증을 해야한다. `gh auth login` 명령어를 입력한다. (화살표로 위아래 움직일 수 있으며, GitHub.com의 계정으로 인증할 것이기 때문에, 엔터를 누른다.)
![image](https://user-images.githubusercontent.com/56383948/262601186-d9fd4d1a-e040-477c-a373-6fe7d1768856.png)

<br/>

4. HTTPS, SSH 둘 중 어떤 프로토콜을 선호하는지 정하고, 엔터를 누른다.
![image](https://user-images.githubusercontent.com/56383948/262601581-96ccaefd-97a8-4301-bc93-e2c58925fb5a.png)

<br/>

5. SSH를 누르게된다면, ssh key를 만들 때 사용한 .pub 파일을 선택한다.
![image](https://user-images.githubusercontent.com/56383948/262601922-4bb92592-85ce-4814-b316-d53323b78a86.png)

<br/>

6. ssh Key를 만들 때 사용한 ssh key title을 입력한다.
![image](https://user-images.githubusercontent.com/56383948/262602422-48100d28-d8d5-40cf-b71e-532f1b7b9aac.png)

<br/>


7. 어떤 방식으로 로그인을 할지 선택한다.
![image](https://user-images.githubusercontent.com/56383948/262602499-dd168063-fc75-4ceb-a145-672e194bcb25.png)

<br/>

8. 웹 브라우저로 인증을 한다고 하고, 엔터를 입력하면 인증 브라우저가 뜨는데, 인증 코드를 입력한다.
![image](https://user-images.githubusercontent.com/56383948/262603459-cafa2282-066b-4e80-886f-62df6c679c23.png)
![image](https://user-images.githubusercontent.com/56383948/262603680-742d3901-e6af-4f6d-bd82-40e5c390b56e.png)
![image](https://user-images.githubusercontent.com/56383948/262603755-6bcd7b55-17ef-42fb-98f5-23939f3233ac.png)
![image](https://user-images.githubusercontent.com/56383948/262603834-f6c6b78f-e34e-4f2c-92fe-a6d7e5180526.png)

<br/>

9. 인증이 완료되었다면, `gh repo clone { }` 명령을 이용해서, 잘되는지 확인해본다.
![image](https://user-images.githubusercontent.com/56383948/262604269-ea850a12-b1c3-423a-b490-df24d73c7564.png)

<br/><br/>

## GUI 버전 관리 환경 구축

---

<br/>

### sourcetree 구축

<br/>

`sourcetree는 git을 명령 줄 대신 GUI를 제공해서, 저장소의 버전 관리 작업을 더 쉽게 만들어준다.`

<br/>

1. [sourcetree](https://www.sourcetreeapp.com/)에 들어가서, 자신에게 맞는 버전을 다운받는다.
![image](https://user-images.githubusercontent.com/56383948/262606655-7693171f-8a1b-4c52-89a9-9d0dd7a7ec91.png)

<br/>

2. 설치 후 실행을 하고, bitbucket은 사용하지 않으므로 넘어간다. config 기본 설정은 미리 해두면 편하다.
![image](https://user-images.githubusercontent.com/56383948/262611111-c5542831-62f8-4e0d-9342-8fe01d6c4c61.png)

<br/>

3. 이후, 계정을 눌러서 호스트를 Github으로, 인증 방식을 OAuth를 선택한다.
![image](https://user-images.githubusercontent.com/56383948/262611355-7d9b57a9-2757-435a-91d7-7777614159f2.png)

<br/>

4. 계정 연결로 Github과 sourcetree를 이어준뒤, HTTPS 프로토콜을 이용해 간단하게 구축한다.
![image](https://user-images.githubusercontent.com/56383948/262612079-f347861a-62c2-407f-ab2f-37fad0b797c1.png)

<br/>

5. 계정을 클릭하고 디폴트 설정을 해주면, 계정에 있는 repo가 원격에 뜨게 된다.
![image](https://user-images.githubusercontent.com/56383948/262612260-502ac3bf-8762-4561-a89d-da1103008131.png)

<br/>

6. sourcetree를 이용해서, 원격 저장소의 clone이 잘 되는지 확인한다.
![image](https://user-images.githubusercontent.com/56383948/262612447-441d4c69-006d-41e3-a1c2-97a6e3027529.png)
![image](https://user-images.githubusercontent.com/56383948/262612718-735c23d4-ee66-491b-8814-866794c14b89.png)

<br/><br/>

### gitkraken 구축

<br/>

`gitkraken은 초보자부터 전문가까지 Git 작업을 더 편리하게 만들기 위해, 탄생한 도구이다. GUI기반으로 sourcetree보다 빠르다. 하지만 Mercurial은 지원하지않는다.`

1. [gitkraken](https://www.gitkraken.com/)에 접속해서, 자신에게 맞는 버전을 다운로드 받는다.
![image](https://user-images.githubusercontent.com/56383948/262616061-10a1ec48-da57-4ff7-a23a-e493b3c24097.png)

<br/>

2. 파일을 설치한 뒤, Sigh in을 클릭한다.
![image](https://user-images.githubusercontent.com/56383948/262616282-3174a984-afa0-4bcd-8f9d-416240e8907d.png)
![image](https://user-images.githubusercontent.com/56383948/262616398-4e66940f-545c-48a2-aef8-26ea010e2619.png)
<br/>

3. GitHub을 클릭하고, GitHub과 Kraken에게 권한을 부여해준다.
![image](https://user-images.githubusercontent.com/56383948/262616460-4f665088-87f6-4c60-9480-822e379d0c16.png)
![image](https://user-images.githubusercontent.com/56383948/262616633-bd4f4ed4-3b93-4b3f-942c-4e5f832f0c0f.png)
<br/>

4. GitHub에서, 원격 저장소를 선택해서 클론하거나, URL을 통해 클론할 수 있다.
![image](https://user-images.githubusercontent.com/56383948/262616852-1b6caacb-2e09-43a8-a732-1d7e7916bd26.png)
![image](https://user-images.githubusercontent.com/56383948/262617046-d92d0374-d175-43f6-83ad-99ded5c68bc4.png)
<br/>

5. 이미 Clone한 프로젝트가 있다면, 열어서 관리할 수 있다.
![image](https://user-images.githubusercontent.com/56383948/262617253-d27e6dc3-b2db-4a30-9709-dc64045975cb.png)



<br/><br/>