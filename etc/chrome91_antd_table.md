# Chrome 91에서 table이 깨지는 경우 

Chrome version : 버전 91.0.4472.77(공식 빌드) (x86_64)
ant.design : 3.26.16

<br>

ant.design(이하 antd)에서 제공하는 Table이 페이지 크기를 벗어나서 일부 영역이 보이지 않는 이슈가 발생했다. <br>
해당 이슈가 재현되는 환경을 확인해보니 크롬 유저 중에서 91 버전에서만 발생했다. (크롬 외 다른 브라우저는 제외해둔다.)  <br>


antd의 공식 github 페이지에서 issue 탭은 꽤나 활발하다.  
antd를 쓰다가 궁금하거나 이상하다 싶은 부분이 있다면 issue 탭으로 달려가서 검색해보자. 대부분 해결할 수 있다. 

 <br>

> antd github: https://github.com/ant-design/ant-design 

_심심찮게 중국어를 번역해보는 경험도 해볼 수 있다._

<br>

본론으로 다시 돌아와서.  <br>
크롬 91 버전이 이슈 발생일로부터 10일(May 25, 2021)전에 릴리즈되었지만,  <br>
그로부터 3일 뒤인 28일에 같은 이슈를 경험한 또 다른 개발자가 issue를 올려줘서 문제를 쉽게 해결할 수 있었다.   <br>

 <br>

### https://github.com/ant-design/ant-design/issues/30755

_무려 크롬 개발자분께서 직접 등판했다 !_

 <br>
 

> The problem is min-width CSS property.  <br>
> Before Chrome 91, min-width was ignored on COL elements. 91 no longer ignores it.
 

크롬 91부터는 COL 관련 태그에 관해서 min-width 속성을 무시하지 않고 적용한다.  <br>
antd의 Table 컴포넌트 내부에서 설정된 min-width 속성으로 인해 resize가 제대로 동작하지 않았던 것이다.  <br>


위 이슈를 통해 antd 측에서 해당 속성을 제거한 `rc-table@7.15.2`버전을 빠르게 제공하고 있다.  <br>
해당 버전 사용이 어렵다면 아래 CSS 속성을 추가하여 임기응변할 수 있다.  <br>

