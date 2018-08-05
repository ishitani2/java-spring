자바 #1
===========================================

# 1. 자바 클래스 로더
## 1.1 자바 클래스 로더란
	자바클래스를 자바 가상 머신(JVM)으로 동적 로드하는 자바 런타임 환경의 일부이다. 
	JVM내로 자바 클래스를 Load하고 Link를 통해 적절히 배치한다.
	로딩은 요청이오면 단 한차례 수행되며, 호출될 때까지는 로드되지 않는다.


## 1.2 클래스 로더 계층 구조
	JVM이 시작되면 Bootstrap Class Loader, Extenstion Class Loader, System Class Loader 로더가 사용된다.
* Bootstrap Class Loader
	최상위 계층
	Native Code로 구현되었다.
	<JAVA_HOME>/jre/lib 디렉토리에 위치한 핵심 자바 라이브러리들을 불러들인다.
	Object Class를 포함한 Java API를 로드한다.
* Extension Class Loader
	Bootstrap Class Loader를 부모로 한다.
	<JAVA_HOME>/jre/lib/ext 또는 java.ext.dirs 시스템 속성에 지정된 기타 디렉토리의 코드를 로드한다.
	기본 JAVA API를 제외한 확장 클래스들을 로드한다.
* System Class Loader
	Extension Class Loader를 부모로 한다
	CLASSPATH 환경변수, java.class.path에 위치한 Class를 로드한다.
* User-defined Class Loader
	사용자가 직접 클래스 로더를 생성할 수 있다.
	System Class Loader를 부모로 가지거나 다른 User-defined Class Loader를 부모로 가질 수 있다.

## 1.3 클래스의 로딩
	클래스가 이미 Load되어 있으면 이를 반환
	로드 되어 있지 않으면 Parent에 위임
	Parent가 찾지 못하면 Current Class Loader에서 Class 찾음

# 2. 자바 메모리 구조
## 2.1 Class Area (=Method Area, Static Area)
	클래스 파일의 바트이 코드가 로드되는 곳
	메인 메소드에서 사용되는 클래스와 static 변수 또한 이 메소드 영역에 로드됨
	이 곳에 저장되는 코드는 프로그램의 흐름을 구성하는 바이트코드이다
	자바 컴파일러에의해 컴파일되는 코드들이 로드된다.

## 2.2 Stack Area
	지역변수와 매개변수가 저장된다.
	프로그램의 실행과정에서 임시로 할당되고 사용이 끝나면 바로 소멸되는 것들이 저장된다.
	기본(원시)타입 변수는 스택 영역에 직접 값을 가진다.
	
## 2.3 Heap Area
	흔히 코드에서 new명령을 통해 생성된 인스턴스 변수가 놓인다.
	메소드 호출이 끝나도 사라지지않고 가비지 컬렉터에 의해 지워지거나 JVM이 종료될 때까지 유지된다.

## 2.4 Native Method Stack Area
	자바 외 언어로 작성된 네이티브 코드를 위한 Stack
	네이티브 메소드의 매개변수, 지역 변수 등을 바이트 코드로 저장한다.

## 2.5 PC Register
	현재 수행 중인 JVM 명령 주소를 가진다.

# 3. AOP (Aspect Oriented Programming) 용어
## 3.1 Joinpoint
	Advice를 적용 가능한 지점, 메서드 호출, 필드 값 변경등이 해당됨
	스프링은 Proxy를 통해 AOP를 구현하기 때문에 메소드 호출에 대한 Joinpoint만 지원한다.

## 3.2 Pointcut
	Joinpoint의 부분 집합
	실제로 Advice가 적용되는 Joinpoint

## 3.3 Advice
	언제 공통 관심 기능을 핵심 로직에 적용할 지를 정의한다.
		Before Advice : 메소드 호출 전
		After Returning Advice: 메소드가 익셉션 없이 실행된 이후
		After Throwing Advice: 익셉션이 발생한 경우
		After Advice: 메소드 실행 후, 익셉션 여부와 관계없이 실행
		Around Advice: 실행 전, 후 또는 익셉션 발생 시점에 공통 기능 실행하는데 사용
## 3.4 Weaving
	Advice를 핵심 로직 코드에 적용하는 것

## 3.5 Aspect
	여러 객체에 공통으로 적용되는 기능
	예> 트랜잭션이나 보안

# 4. Java Proxy
	다른 무언가와 이어지는 인터페이스의 역할을 하는 클래스
	네트워크 연결, 메모리안의 커다란 객체, 파일, 리소스등과의 인터페이스 역할을 할 수 있다.
	기본적으로 직접 Target 클래스를 인스턴스로 만들지 않으며 인터페이스를 통해 위임, target  클래스의 메소드를 제어
	Proxy클래스에는 target 클래스의 프록시 객체를 반환하는 메소드가 있으며, target 클래스 로더, target 클래스 인퍼테이스, 구현 클래스를 파라미터로 받는다.
	Target 클래스가 Interface일 때만 사용할 수 있다.
	
# 5. Thread와 공유 객체
## 5.1 Thread
	프로그램의 실행 흐름, 프로그램을 구성하고있는 실행 단위
	기본적으로 멀티 태스킹을 위한 기본 도구
	동시에 실행되어야하는 개별적인 모듈
	기본적으로 자바는 멀티 쓰레드

## 5.2 공유 객체
	싱글 쓰레드 환경에서는 스레드가 객체를 독차지해서 사용하지만 자바처럼 멀티 쓰레드 환경에서는 객체를 공유해서 사용해야 한다.
	즉, 쓰레드 1과 쓰레드 2가 동시에 객체를 공유해서 사용하는 경우가 있는데 이를 공유 객체라고 한다.
	이로 인한 부작용을 막기 위해서는 동기화 작업이 필요하다.
	동기화 방법은 동기화 하고 싶은 메소드에 synchronized 키워드를 붙임


# 6. Gradle, Maven
## 6.1 Gradle
	2012년 Ant와 Maven의 장점을 모아 출시
	Android OS의 빌드 도구로 채택
	유연한 범용 빌드도구
	Maven변환 가능
	멀티 프로젝트에 사용하기 좋음
	Apache Ivy에 기반한 강력안 의존성 관리
	파일없이 연결되는 의존성 관리 지원
	그루비 문법 사용
	빌드를 설명하는 풍부한 도메인 모델

## 6.2 Maven
	2004년 Apache 프로젝트 출시
	Ant의 불편함 해소와 기능 추가
	빌드를 쉽게 도와줌
	pom.xml을 이용한 정형화된 빌드 시스템
	뛰어난 프로젝트 정보 제공
	개발 가이드 라인 제공(테스트, Unit Test 커버)
	새로운 기능을 쉽게 설치/업데이트 가능


	