# CLI, GUI 버전 관리 환경 구축

- [ ] CLI 버전 관리 환경 구축
  - [x] git 구축
  - [ ] gh 구축
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