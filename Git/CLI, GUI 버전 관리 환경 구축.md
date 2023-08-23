# CLI, GUI 버전 관리 환경 구축

- [ ] CLI 버전 관리 환경 구축
  - [x] git 구축
  - [x] gh 구축
- [ ] GUI 버전 관리 환경 구축
  - [ ] sourcetree 구축
  - [ ] gitkraken 구축
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

<br/>

### gh 구축

<br/>

`gh는 GitHub CLI를 말하며, 터미널 환경 또는 명령 프롬프트에서 사용이 가능하게 해주는 GitHub 공식 도구이다. GitHub 웹페이지에서 하던 이슈, PR, 저장소 생성 등 기타 GitHub관련 작업을 수행할 수 있다.`

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

<br/>

## GUI 버전 관리 환경 구축

---

<br/>

### sourcetree 구축

<br/>

<br/>

### gitkraken 구축

<br/>

<br/>