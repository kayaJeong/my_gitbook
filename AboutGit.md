#AboutGit



1. pull request 에 대해서 알아보고 사용법 정리하기, 
2. HEAD 에 대한 개념을 정리하고 내 소스폴더를 오리진의 특정 커밋을 기준으로 다시 초기화 하는 방법(reset)을 알아보고 정리하기, 
3. gitflow 라는 것이 뭔지 그냥 알아보기 (정리는 필요치 않으며 branch를 어떻게 활용하는지를 잘 알아보시면 됩니다.)


1. Pull Reuqest 
- push 권한이 없는 프로젝트에 기여할 때 내가 작성한 코드를 프로젝트에 합치고 싶을 때 사용

1-1. 다른 사람의 원격레파지토리에서 folk로 내 레파지토리에 가져오기 (다른사람 원격 레파지토리 -> 내 원격레파지토리) 

1-2. 내 원격레파지토리 -> 내 로컬레파지토리 클론할 폴더에서 git bash 로 클론함 
git clone 내원격레파지토리주소 //내 원격레파지토리 클론 클론한 폴더 안에 들어가서 git bash로 명령어 치기 
git remote add 별명 다른사람원본원격레파지토리주소 //나중에 git pull 하기 위해서 연결함 
git remote -v //현재 연결되어 있는 원격 저장소 확인 : //내원격레파지토리-로컬, 다른사람레파지토리-로컬 연결 확인 
git remote remove 별명 //연결되어 있는 로컬레파지토리와 원격레파지토리와 연결 끊기 

1-3. 브랜치를 만들어서 작업 
git branch 브랜치이름 // 브랜치이름으로 브랜치 생성 
git checkout 브랜치이름 //브랜치이름으로 이동 
git branch //브랜치 리스트 

1-4. 코드 작업 후 내 원격레파지토리에 git push하기 
git add . 
git commit -m "third commit" //변경사항 히스토리 만들기 
git push origin 브랜치이름 // 내원격레파지토리의 브랜치에 변경사항 push 

1-5. 내 원격레파지토리 -> 다른사람원격레파지토리에 Full Request 하기 
내 원격레파지토리에서 상단에 Compare&pull request 버튼 누르고 어디서 어디로 full request를 할 것인지 확인하고 requeset 하기 

1-6. 다른사람은 깃허브에서 Full Request 확인 다른 사람의 원격레파지토리에서 full request 온 것을 확인하고 merge할 것인지 거절할 것인지 확인하면 됨. 

1-7. 업데이트 된 다른사람 원격레파지토리 pull 하기
![pic2](/img/pic2.jpg)


![pic1](/img/pic1.jpg)


2. HEAD 
Git log 찍었을 때 HEAD는 특정 커밋을 가리킨다
모든 브랜치는 HEAD값이 존재하고 HEAD는 해당 브랜치의 마지막 커밋을 의미한다
브랜치를 변경하면 변경한 브랜치의 가장 최신 commit을 가리킴 
 
현재 HEAD -> develop 은 마지막 커밋은 develop 브랜치에서 이뤄짐 
HEAD -> 브랜치네임

Git log 탈출 하기 : q 


특정 commit 으로 이동
Git checkout HEAD~번호  //현재 HEAD가 위치한 곳에서 번호번째 뒤의 커밋으로 이동
Git checkout 현재브랜치이름  // 현재브랜치로 돌아오기 

Git reset hard HEAD~ // 바로 직전 커밋으로 이동 

특정 커밋을 기준으로 초기화하기

 
Git reset --hard HEAD^ // 바로 직전 커밋의 워킹디렉토리로 되돌리기
Git reset --mixed HEAD^ // 바로 직전 커밋의 스테이징에어리어로 되돌리기
Git reset –soft HEAD^ // 바로 직전 커밋의 local repo로 되돌리기 

