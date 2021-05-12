# visualVM intellij 연동해서 Thread 모니터링해보기

1. visualVM 다운로드
- [visualVM 공식 홈페이지](https://visualvm.github.io/)에서 zip 파일 다운로드
- zip 파일 압축을 해제하면 “visualvm_버전” 파일이 생성되는데, 파일 통째로 /Library/Java 하위로 이동 ex) /Library/Java/visualvm_207

2. VisualVM Launcher Plugins 설치
- Intellij > Plugins > Browe repositories 에서 visualVm 검색
- VisualVM Launcher  Install
- Install 완료 후 intellij 재실행

3. VisualVM Launcher 환경변수 설정
- Preferences > Other Settings > VisualVM Launcher
- VisualVM executable, JDK home 설정을 변경해준 후 Apply 

4. VisualVM을 이용해 실행
- 다음과 같이 Run With VisualVM, Debug With VisualVM 아이콘이 생성됨
- With VisualVM으로 실행
- VisualVm Application이 자동실행
- Local 탭에 Tomcat (pid: ~ ) 와 같이 실행된 톰캣 정보를 확인
