#[Spring Boot] 의존성 버전 사용자화 하는 방법

Spring에서 ES(Elastic Search) 통신을 편리하게 사용하기 위해 만들어진 커스텀 모듈을 사용하기 위해 의존성을 추가하던 중 원치 않는 버전의 ES 모듈들이 임포트된다는 것을 알게 되었다. 커스텀 모듈 내에는 임포트하고자 하는 ES 버전이 명시가 되어 있었음에도 불구하고.

Spring Boot를 사용하다보면 의존성을 받아오기 위해서는 2가지 방법이 있다.

<parent>
<dependencyManagement>

이 두 개의 태그 중 하나를 사용하면 된다.

내가 사용하던 레포에서는 <dependencyManagement>로 spring-boot-dependencies가 선언이 되어 있었고 이로 인해 원치 않는 버전이 임포트되고 있었다. spring-boot-dependencies 내부를 살펴보면 <properties>와 <dependencyManagement>를 이용해 추가되는 의존성 버전을 관리하고 있음을 확인할 수 있다.

아래 링크를 참조하자
https://docs.spring.io/spring-boot/docs/current/reference/html/appendix-dependency-versions.html


특정 의존성의 버전을 변경하려면 아래와 같이,
변경하고 싶은 버전을 properties를 이용해서 명시하고 해당 의존성을 추가해주면 overriding 된다.

<parent>

<properties>
	<elasticsearch.version>6.4.3</elasticsearch.version>
</properties>

...

<dependencies>
	<dependency>
		<groupId>org.elasticsearch</groupId>
		<artifactId>elasticsearch</artifactId>
		<version>${elasticsearch.version}</version>
	</dependency>
</dependencies>
<dependencyManagement>를 사용하여 의존성 관리를 하는 경우에는
아래와 같이 spring-boot-dependencies 모듈 이전에 설정을 추가해준다.

<dependencyManagement>
	<dependencies>
		<!-- 추가하고자 하는 모듈 및 버전 설정 -->
		<dependency>
			<groupId>org.elasticsearch</groupId>
			<artifactId>elasticsearch</artifactId>
			<version>6.4.3</version>
		</dependency>
		<!-- spring-boot-dependencies -->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-dependencies</artifactId>
			<version>2.2.6</version>
			<type>pom</type>
			<scope>import</scope>
		</dependency>
	</dependencies>
</dependencyManagement>
<dependencyManagement>의 경우, <parent>에서 설명한 방법과 같이 설정해줘도 정상 동작한다.

그 이유는 무엇일까 ?

참고
https://docs.spring.io/spring-boot/docs/2.4.3/maven-plugin/reference/htmlsingle/#using