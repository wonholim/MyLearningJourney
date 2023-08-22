# Docker로 Jenkins 설치하기

---

<br/>

1. Docker를 먼저, 설치한다. (Docker 공식 사이트에서 Docker Desktop을 설치하는 것을 추천한다.)
    ![image](https://user-images.githubusercontent.com/56383948/262414931-42b6aa6a-8066-4ee7-a1fd-e6678b1ce4e0.png)

<br/>

2. docker -v 명령어를 입력해서, docker가 올바르게 설치되었는지 확인한다.
![image](https://user-images.githubusercontent.com/56383948/262415533-673bb33f-32fe-4699-9aaa-42821a58c049.png)

<br/>


3. Docker Hub에 가입하지 않았다면, 가입을 하는 것을 추천한다.
![image](https://user-images.githubusercontent.com/56383948/262415315-d095c5ad-2a2b-4ff6-ba4b-b6ccf2c942d7.png)

<br/>

4. docker Hub에서 jenkins를 검색하고, docker pull 커맨드를 알아낸다. (docker pull jenkins/jenkins) [jenkins](https://hub.docker.com/r/jenkins/jenkins)
![image](https://user-images.githubusercontent.com/56383948/262416142-10de1c5b-d05e-4ef1-8a9e-d114fcbf8acc.png)

    ```
    docker pull jenkins/jenkins (latest version)
    To use the latest LTS: docker pull jenkins/jenkins:lts-jdk11
    To use the latest weekly: docker pull jenkins/jenkins:jdk11
    ```

<br/>

5. docker에서 jenkins를 사용하는 명령어는 github jenkins 공식 계정에서 알아볼 수 있다. [jenkins](https://github.com/jenkinsci/docker)
![image](https://user-images.githubusercontent.com/56383948/262416757-9e1436f2-edc9-4841-bed6-29abd996cdd7.png)

    ```
    docker run -p 8080:8080 -p 50000:50000 --restart=on-failure jenkins/jenkins:lts-jdk11
    docker run -p 8080:8080 -p 50000:50000 --restart=on-failure -v jenkins_home:/var/jenkins_home jenkins/jenkins:lts-jdk11
    docker run -d -v jenkins_home:/var/jenkins_home -p 8080:8080 -p 50000:50000 --restart=on-failure jenkins/jenkins:lts-jdk11
    ```
    - `p` : publish 옵션, 컨테이너 바깥의 포트가 내부의 포트에서 어떤 것을 사용할지 정한다.
    - `jenkins/jenkins:lts-jdk11` : jenkins/jenkins jenkins 계정의 jenkins repo, lts-jdk11 TAG의 역할이지만, 대부분은 버전을 가리킴
    - `-v jenkins_home:/var/jenkins_home` : 마운트 작업, docker 내부에 저장된 데이터를 저장하기 위해서, 사용한다.
    - `name` : 이름을 추가한다. 없을시 랜덤 값으로 지정이된다.

<br/>

6. 자신에게 맞는 명령어를 이용해서, 컨테이너화시킨다. 만약 이미지가 없어도, docker가 자동으로 이미지를 pull하고, 컨테이너화 시켜준다.
![image](https://user-images.githubusercontent.com/56383948/262417235-2e201968-6ca5-4e34-bd93-2b618bb79d03.png)

<br/>

7. docker ps 명령을 이용해서, status를 확인한다.
![image](https://user-images.githubusercontent.com/56383948/262417569-1118d50c-4ced-4556-a263-55f179bd8c8c.png)

<br/>

8. `http://yourIP:yourPort` 로 접속한다. 나의 경우, 로컬에서 만들었고, 8080포트로 이어주었기에 `http://localhost:8080`으로 접속하면 된다.
![image](https://user-images.githubusercontent.com/56383948/262418583-8c44a9a9-ae9e-462a-a1a2-6e8affed574a.png)
  - 접속을 하면, 비밀번호를 입력하라고 뜨는데, `docker logs containerName`을 입력하면, jenkins 설치 과정에서 만들어진 비밀번호를 볼 수 있다.
  ![image](https://user-images.githubusercontent.com/56383948/262419088-224466f5-5804-4751-938b-00d3d5d1ddca.png)

<br/>

9. 이후 jenkins 플러그인을 자신에게 맞게 설치를 한다. 잘 모르겠다면, 왼쪽을 눌러서, 다 설치하자.
![image](https://user-images.githubusercontent.com/56383948/262419551-1213f680-d2ce-4161-824f-c3601ce8c827.png)

<br/>

10. 설치 이후, 어드민 계정을 생성한다.
![image](https://user-images.githubusercontent.com/56383948/262419769-dd7addef-c15b-4c6f-be4c-d72feb2958bb.png)

<br/>

11. 계정 생성이 완료되었다면, jenkins를 시작한다.
![image](https://user-images.githubusercontent.com/56383948/262420064-75553fc6-819b-4224-929c-e43b5e6d219d.png)
