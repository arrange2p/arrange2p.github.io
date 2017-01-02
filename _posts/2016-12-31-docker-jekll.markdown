---
layout: post
title:  "github page 만들기 (docker, jekyll)"
date:   2016-12-31
comments: true
categories: jekyll
---

### github 개인 페이지 만들기
사내 게시판에서만 공유를 하다가 그래도 오픈 된곳에서 공유를 하게 되면 좀 더 열심히 작성을 하지 않을까해서 적당한 블러그들을 찾아 보고 있었습니다.   
포털 블로그나 워드 프레스를 이용해서 만들 수 도있지만 글과 코드에 집중하기에는 너무 번거러운것들이 많이 있어서 github를 선택하게 되었습니다.   
- 장점
: - 도메인과 서버비용이 없다.   
: - 개발관련 글들은 시간이 지남에 따라서 변경 되는 내용이 많은데 변경 내역을 확인 할 수 있다.
: - markdown을 이용하기 때문에 글에 좀 더 집중 할 수 있다.
: - 코드 공유하기가 쉽다(?)

- 단점
: - 처음 시작 할때 진입 장벽이 있다. (개발자 아니면 사용하기 쉽지 않을것 같습니다.)
: - ruby, jekyll 등 새로운 기능에 익숙 해져야 합니다.
: - windows 사용자라면 살짝 귀찮을 수 있을 듯합니다.


### github 주소 발급
1. github 계정만들기
2. new repository 추가 {본인계정명}.github.io
3. github settings에 보면 아래와 같이 나오면 생성 된것이다.
> Your site is published at https://본인계정명.github.io
4. github  설정 완료

### jekyll 서버는 어디에 설치 할까?
사실 이부분에서 고민을 많이 하게 되었는데 처음에는 'AWS Free tier'을 사용 하고 있었기 때문에 여기에 설치를 하려고 했고 했었다.     
그런데 프리티어가 끝나면(?) 돈을 내고 써야 하는데 그러면 github을 이용하는 의미가 없어진다.   
그럼 local에 설치하자!!    

앗,, 근데 나는 윈도우다.... 아무래도 가상 서버가 필요한데,,      

그럼 VM을쓸까? docker 쓸까?      
docker 결정

### windows 에서 docker 설치
docker 검색 하면 많이 나온다. 여기서 따로 설명은 안하겠다.    
본인이 리눅스 계열을 사용 한다면 별다른 어려움 없이 사용 할 수 있을것이다.    
대부분 리눅스 명령어 앞에 docker만 넣으면 쉽게 사용 할 수 있다.    
내가 사용하는 서버는 리눅스지만 컴퓨터는 윈도우이다.   

리눅스에서 설치하는 방법은 다음번에 설명 하고 이번에는   
윈도우에서 설치하는 방법을 설명 해보자.

1. Windows 7, 10 두개다 설치가 되긴 된다.   

2. 아래에서 다운을 받으면 된다.
 > [docker-toolbox](https://www.docker.com/products/docker-toolbox "도커툴박스"){: target="_blank"}

3. 설치 도중 windows에서 가상 머신이 지원 안된다면서 설치가 안되는 경우가 있는데 이런경우   
오류 메시지대로 따라가서 윈도우 파일 다운 받아서 설치 하면된다.  <sub>(정상적으로 설치 했는데 안되면 컴퓨터를 재 부팅 해보자.)</sub>

4. 바탕화면  docker quickstart 아이콘이 보이면 정상적으로 설치 된것이다.
  * 실행이 안되는 경우가 있는데 docker 설치할때 VM웨어도 같이 설치 하는지 물어 본다 이경우 없으면 설치 하고 있으면 안해도 된다.
  * 다만 있는 경우에 또 설치 하면 오류가 발생 한다.
  * VM 버전이 오래된경우도 오류가 발생 할 수 도 있다.

### docker에 jekyll 설치
* Dockerfile을 이용하여 jekyll 서버를 만들 수도 있지만 그건 다음에 설명하기로 하자.
* Dockehub에서 jekyll image를 받아서 사용 해보자.   
  1. docker도 github 처럼 이미지를 모아두는 곳이 있다.  
   [hub.docker.com](https://hub.docker.com/  "도커허브"){: target="_blank"}      
   검색해 보면 여러가지가 있는데 나는
   > <https://hub.docker.com/r/jekyll/jekyll/> 사용 했다.   

  2. 이제 부터 docker images에 따라서 사용 하는 방법이 조금씩 다르니 dockerfile 을 보고 적용 해보자

  3. Docker Quickstart Terminal 실행  

  ![docker 시작화면]({{ site.url }}/downloads/dockerinit.png)

  * 위와 같은 화면이 나오면 정상적으로 설치가 완료된것이다.
  * 다음 명령어를 입력 해보자.
    - '-p4000:400' 포트설정, jekyll 기본포트는 4000 인데 외부와 연결 하기 위해서이다.
    - '-v host디렉토리:guest디렉토리' 이부분을 좀 **주의** 깊게 봐야 한다.  
    >*docker 옵션 중 하나인데 외부 저장소와 연결이 가능 하다.   
    나는 윈도우에서 atom에디터와 연결 하기 위해서 윈도우 디렉토리와 도커내부 디렉토리를 연결 했다.  
    윈도우에서 디렉토리 만들시 권한을 조정해줘야 하고 '//' 두개로 시작 한다는 것도 잊지 말자.*
  ~~~
  docker pull jekyll/jekyll  
  docker run -i -t -p4000:4000 -v //c/Users/jekyll:/srv/jekyll –name jekyll jekyll/jekyll /bin/bash  
  ~~~

  * docker 종료후 다시 시작 할 경우(docker stop jekyll)
  > docker 사용법을 기본적으로 알고 있다는 전제하에서 작성 되었다. 그러므로 다소 이해가 안되는 부분이 있을 수도 있다.  
  - 여기서 주의 할 점음 jekyll serve 실행 할때 jekyll 테마가 설치된 곳에서 시작을 해야 동작 된다.   
  - jekyll serve 실행 후 글 을 바로 확인 하기 위헤서 jekyll serve '--watch' 명령어를 사용 하기도 하는데   적용이 안될시 '--force_polling'을 사용하도록 하자.
  ```
  docker start jekyll   
  docker exec jekyll sh -c "cd /srv/jekyll/arrange2p.github.io && jekyll serve --force_polling"
  ```
  *docker 사용법은 다음에 설명*

### jekyll 테마 사용 및 github 저장하기
- jekyll 테마를 설정 해야 하는데 설정 안해도 기본 테마로 적용 되기때문에 선택 사항이다.    하지만 테마 적용을 안 할꺼면 왜 서버까지 설치 할 필요는 없다.
- 테마를 본인이 만들거나 수정 할 수도 있지만 되어 있는 테마를 무료로 가져다 써보자.
1. <http://jekyllthemes.org/>{: target="_blank"} 여기서 원하는 테마를 고를 수 있다.   
  (jekyll은 정적 페이지기때문에 여러가지 기능을 추가 하려면 또 다르게 설정 해야 되므로 요구사항을 잘 보자.)
2. 여기서 본인 github 계정으로 fork 한다.
3. 다른 방법도 있지만 fork한 repository이름을 아까 설정한 {본인계정}.github.io 변경한다. 그 전에 생성한 repo는 삭제 해야 한다.(이방법이 편한다.)
4. docker jekyll 디렉토리에서 git clone 한다.
5. 포스트 작성 및 사용 방법은 <http://jekyllrb-ko.github.io/docs/posts/>{: target="_blank"} 자세히 나와있다.


### 보충 설명
github, jekyll, docker 3가지를 사용했기 때문에 내용이 좀 많다.   
전체적인 흐름 위주로 작성 했다고 보면 된다. 피드백을 받고 내용 추가 및 수정할 예정이다.
