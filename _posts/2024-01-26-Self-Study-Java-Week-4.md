---
title: (JAVA) 혼공자바 4주차 - 상속
author: noa
date: 2024-01-26 17:25:00 +0900
categories: [Study, JAVA]
tags: [JAVA, 혼공단]
teaser: /assets/img/posts/2024-01-26-Self-Study-Java-Week-4/thumb.png
---

## 상속

* 상속: 기존의 클래스를 확장해 새로운 클래스를 만드는 것  

자바에서의 상속은 다음의 두 가지 특성을 가지고 있다.   
1. 하나의 부모 클래스만 상속할 수 있다,
2. private 접근 제한을 갖는 필드, 메소드는 상속받을 수 없다.  

상속 받은 자식 클래스 생성자를 실행하면 첫줄에 부모 클래스 생성자를 먼저 실행해 객체를 생성하고 이를 자식 클래스 객체가 참조한다.  
부모클래스에 기본 생성자가 없거나 기본 생성자 이외의 생성자를 사용하고 싶다면 <span style="color:red">super()</span> 함수를 자식클래스 생성자 첫 줄에 입력해주어야 한다.  

**✔️확인 문제**  
4. Parent 클래스를 상속해 Child 클래스를 다음과 같이 작성했는데, Child 클래스의 생성자에서 컴파일 에러가 발생했습니다. 그 이유를 설명해보세요.  
```java
package sec01.test04;

// Parent 클래스
public class Parent {
	public String name;
	
	public Parent(String name) {
		this.name = name;
	}
}
```  

```java
package sec01.test04;

// Child 클래스
public class Child extends Parent{
	private int studentNo;
	
	public Child(String name, int studentNo) {
		this.name = name;
		this.studentNo = studentNo;
	}
}
```  
<span style="color: #f08080">
▶ Parent 클래스는 생성자를 따로 선언해 기본 생성자가 없는 상태인데, 생성자를 호출해주지 않아 오류가 발생한다.</span>

## <span style="background-color: #ffdce0; color: black">메소드 재정의
</span>

* <span style="background-color: #ffdce0; color: black">Overriding</span>
(메소드 재정의): 부모 클래스에서 정의된 메소드를 자식 클래스에서 재정의 하는 것  

* 동일한 이름이지만 매개변수가 다른 
<span style="background-color: #fff5b1; color: black">Overloading</span>
과는 달리, 
<span style="background-color: #ffdce0; color: black">Overriding</span> 
되는 메소드는 동일한 매개변수, 리턴 타입을 가진다.  

* <span style="background-color: #fff5b1; color: black">Overloading</span>
과 
<span style="background-color: #ffdce0; color: black">Overriding</span>
은 모두 동일한 이름을 가진다는 특징이 있다.  

Overriding을 사용하려면 다음과 같이 Overriding Method 앞에 ```@Override``` 를 붙여주면 된다.

```Java
public class Computer extends Calculator{
	@Override
	double areaCircle(double r) {
		return Math.PI * r * r;
	}
}
```

* 메소드가 재정의되면 기존의 부모 객체 메소드는 숨겨진다.(= 자식 객체의 새롭게 정의된 메소드만 사용할 수 있다.)  

* 메소드 재정의로 접근 제한을 더 강하게 재정의할 수 없다.

* 자식 클래스에서 재정의된 기존 부모 클래스 메소드를 사용하려면 ```super.메소드()```를 통해 호출할 수 있다.  
<span style="color: #f08080">
단, 자식 클래스 내에서만 사용 가능</span>  

### final 클래스와 final 메소드

* ```final``` 클래스는 부모가 될 수 없는 최종 클래스  

* 메소드 앞에 ```final```을 붙여주면 Overriding을 수행할 수 없다.

* 아래와 같이 클래스를 처음 정의할 때, 접근 제한자 다음에 final을 작성해 구현할 수 있다.

```java
public final class Member {
}
```  

**✔️확인 문제**  
3. final 클래스, final 필드, final 메소드에 대한 설명입니다. 맞는 것에 O표, 틀린 것에 X표 하세요.  
① 모두 상속과 관련이 있다. (O)  
② final 메소드를 가진 클래스는 부모 클래스가 될 수 없다. (O)  
③ final 메소드는 재정의를 할 수 없다. (O)  
④ final 클래스는 final 필드가 반드시 있어야 한다. (X)  
<span style="color: #f08080">
▶ 필요에 따라 선언을 선택할 수 있다.</span>

## 자동 타입 변환

* 자식 객체는 부모 객체로 자동 타입 변환이 가능  

* 부모 타입 변수에 자식 타입을 대입할 수 있음

```java
SuperCar superCar = new SuperCar()
Car car = superCar;
```

* superCar와 Car는 모두 변수이며, 같은 객체를 참조하고 있음  

* 상속 관계일 때만 가능하며, 2대(부모 클래스의 부모 클래스) 이상이어도 자동 타입 변환이 가능하다.

* 다만, 클래스 내 멤버는 부모 클래스 멤버로만 한정

* **재정의된 메소드는 호출 자식 메소드로 호출된다.**  

✅ 부모 클래스에서 정의된 멤버로만 생성된 객체가, 자식 클래스의 필드를 사용해야 하는 재정의 메소드를 호출하려고 하면 문제가 발생할 수 있기 때문에 주의해야 한다.  

### 다형성

* **자동 타입 변환이 필요한 이유**)  
부모 클래스로부터 생성된 객체의 기능 중 특정 부분을 강화하고 싶거나 성능을 높이고 싶을 때, 기존 부모 클래스를 변경하지 않고 다형성을 이용할 수 있다.  
**+다형성: 동일한 방법으로 서로 다른 종류의 객체를 사용하는 것**  
이러한 다형성은,  
① 자식 클래스는 부모 클래스의 필드와 메소드를 가짐  
② 자식 클래스에서는 부모 클래스의 메소드를 재정의할 수 있음  
③ 자식 타입을 부모 타입으로 변환  
이렇게 세 가지 조건이 충족될 때 가능하다.  
자동 타입 변환이 가능하기 때문에 자식 클래스와 다형성을 사용해 기존의 '클래스'를 바꾸지 않고 성능을 높일 수 있다.  

* 매개변수 타입이 클래스인 경우에는, 해당 클래스의 객체뿐 아니라 자식 객체까지도 매개변수로 사용할 수 있다.  
<span style="background-color: #fff5b1; color: black">
앞서 언급했듯 자식 타입을 부모 타입으로 변환하면 재정의된 메소드를 사용한다.</span>

**✔️확인 문제**  
2. Tire 클래스를 상속받아 SnowTire 클래스를 만들었다. SnowTireExample 클래스를 실행했을 때 출력 결과는?  

```java
package sec02.test02;

public class Tire {
	public void run() {
		System.out.println("일반 타이어가 굴러갑니다.");
	}
}
```

```java
package sec02.test02;

public class SnowTire extends Tire{
	@Override
	public void run() {
		System.out.println("스노우 타이어가 굴러갑니다.");
	}
}

```

```java
package sec02.test02;

public class SnowTireExample {
	public static void main(String[] args) {
		SnowTire snowTire = new SnowTire();
		Tire tire = snowTire;
		// 자동 타입 변환으로 snowTire와 tire가 
		// 참조하는 객체가 동일하다.

		snowTire.run(); 
		tire.run();
		// 따라서 동일한 내용을 출력한다.(메소드 재정의된 결과)
	}
}
```

```cmd
<출력 결과>
스노우 타이어가 굴러갑니다.
스노우 타이어가 굴러갑니다.
```  

### 강제 타입 변환  

* 부모 타입을 자식 타입으로 변환하는 것  

* 자식 타입 -> 부모 타입 변환 후 <span style="background-color: #fff5b1; color: black">
다시 변환할 때</span> 사용 가능  

* 자식 타입을 부모 타입으로 변환 했을 때 부모의 필드, 메소드만 사용해야 하므로, 필요에 따라 강제 타입 변환 할 수 있음  

### 객체 타입 확인

* 부모 변수가 참조하는 객체가 부모 객체인지 자식 객체인지 확인하는 방법을 제시  

다음과 같이 사용한다.

```java
boolean result = 좌항(객체) instanceof 우항(타입)
```
**✔️확인 문제**  
3. A, B, C, D, E, F 클래스가 다음과 같이 상속 관계에 있을 때 다음 빈 칸에 들어올 수 없는 코드는?  
![Desktop View](/posts/2024-01-26-Self-Study-Java-Week-4/fig1.jpg)  
①
```java
B b = new B();
```  
B 타입 변수 b는 B 생성자로 선언될 수 있다.  
②
```java
B b = (B) new A();
```
부모타입을 자식타입으로 변환하는 과정은 강제 타입 변환으로, <span style="background-color: #fff5b1; color: black">
자식 타입이 부모 타입으로 변환된 경우에만 사용할 수 있다.</span>  
따라서 새로 생성한 부모 타입 객체를 자식 타입 변수에 대입할 수 없다.  
③  
```java
B b = new D();
```
자식 클래스 D로부터 객체를 생성해 부모 타입 b에 대입하는 것은 자동 타입 변환으로, 가능하다.  
④  
```java
B b = new E();
```
자식 클래스 E로부터 객체를 생성해 부모 타입 b에 대입하는 것은 자동 타입 변환으로, 가능하다.  
따라서 빈 칸에 들어갈 수 없는 코드는 ②뿐이다.  

## 추상 클래스  

* **실체 클래스**: 객체를 직접 생성할 수 있는 클래스  

* **추상 클래스**: 실체 클래스들의 공통적인 특성을 추출해 선언한 클래스  

* **용도**: ① 동일한 특성을 가리키나 개발자마다 다르게 이름붙인 것들을 통일하기 위함  
② 공통 특성을 모아두고, 차별화된 점만 추가한다면, 개발 속도를 높일 수 있음  

* **선언**: 다음과 같이 ```abstract```를 붙여 선언한다. 추상 클래스로 객체를 생성할 수는 없고, 자식 클래스만 만들 수 있다.
```java
public abstract class Phone
``` 
```java
SmartPhone smartPhone = new SmartPhone("홍길동");
Phone phone = new Phone("홍길동") // Error
```   
**✔️확인 문제**  
1. 추상 클래스에 대한 설명으로 맞는 것에 O, 틀린 것에 X표 하세요.  
① 추상 클래스는 직접 new 연산자로 객체를 생성할 수 없다. (O)  
② 추상 클래스는 부모 클래스로만 사용된다. (O)  
③ 추상 클래스는 실체 클래스들의 공통된 특성(필드, 메소드)으로 구성된 클래스이다. (O)  
④ 추상 클래스에는 최소한 하나의 추상 메소드가 있어야 한다. (X)  
<span style="color: #f08080">
▶ 추상 메소드는 필요에 따라 사용을 선택할 수 있다.</span>


### 추상 메소드와 재정의  

* 하위 클래스가 반드시 재정의하길 바라는 메소드를 추상 클래스에서 작성할 수 있고, 이를 추상 메소드라 한다.  

* 추상 메소드는 추상 클래스와 같이 ```abstract``` 를 통해 선언한다.  
```java
public abstract void sound();
```  

* 추상 메소드를 자식 클래스에서 재정의하지 않으면 컴파일 에러가 발생한다.  

* 추상 타입 변수에도 자동 타입 변환이 적용된다.  

**✔️확인 문제**  
2. 추상 메소드에 대한 설명입니다. 맞는 것에 O표, 틀린 것에 X표 하세요.  
① 추상 메소드는 선언부만 있고, 실행 블록을 가지지 않는다. (O)  
② 추상 메소드는 자식 클래스에서 재정의해서 실행 내용을 결정해야 한다. (O)  
③ 추상 메소드를 재정의하지 않으면 자식 클래스도 추상 클래스가 되어야 한다. (X)  
<span style="color: #f08080">
▶ 추상 메소드를 재정의하지 않으면 컴파일 에러가 발생한다.</span>  
④ 추상 메소드가 있더라도 해당 클래스가 꼭 추상 클래스가 될 필요는 없다. (X)  
<span style="color: #f08080">
▶ 추상 클래스에서 추상 메소드를 선언할 수 있다.</span>  

## 기본 미션  

클래스의 타입 변환에는 자동 타입 변환, 강제 타입 변환이 있다.  

**자동 타입 변환**  
* 자식 타입을 부모 타입으로 변환하는 것이다.  
* 기본적으로 부모 타입의 특성(메소드, 필드)만을 사용할 수 있다.  
* 다만, Overriding된 메소드는 자식 클래스의 메소드를 사용할 수 있다.  
* 동일한 방법으로 다양한 기능의 객체를 생성할 수 있는 다형성의 기반이 되는 개념이다.  
* ```부모타입 변수 = 자식타입객체``` 형식으로 구현할 수 있다.

**강제 타입 변환**  
* 부모 타입을 자식 타입으로 변환하는 것이다.  
* 자동 타입 변환으로 자식 타입 객체를 참조하는 부모 타입에 한해 사용할 수 있다.  
* 일반적인 부모 타입은 자식 타입으로 변환할 수 없다.  
* 주요 사용 목적은 자식 타입에서 선언한 필드, 메소드를 사용하기 위함이다.  
* ```자식타입 변수 = (자식타입) 부모타입객체``` 형식으로 구현할 수 있다.  

## 선택 미션  
07-3의 확인 문제 3번을 풀고, 풀이하기  
3. HttpServlet이라는 추상 클래스가 다음과 같이 선언되어 있습니다.  
```java
package sec03.test03;

public abstract class HttpServlet {
	public abstract void service();
}
```  
다음 클래스를 실행하면 "로그인 합니다.", "파일 다운로드 합니다."가 차례대로 출력되도록 LonginServlet과 FileDownloadServlet 클래스를 선언해보세요.  
```java
package sec03.test03;

public class HttpServletExample {
	public static void main(String[] args) {
		method(new LoginServlet());
		method(new FileDownloadServlet());
	}
	
	public static void method(HttpServlet servlet) {
		servlet.service();
	}
}
```  
풀이: 
```java
package sec03.test03;

public class LoginServlet extends HttpServlet{ 
	// 추상 클래스 HttpServlet을 상속받는다.
	
	@Override // 메소드 재정의를 할 것임을 알린다.
	public void service() { 
		// service() 추상 메소드를 재정의한다.
		System.out.println("로그인 합니다.");  
		// 주어진 문구를 출력한다.
	}
}

```  

```java
package sec03.test03;

public class FileDownloadServlet extends HttpServlet{ 
	// 추상 클래스인 HttpServlet을 상속받는다.
	@Override // 메소드 재정의를 할 것임을 알린다.
	public void service() { 
		// service() 추상 메소드를 재정의한다.
		System.out.println("파일 다운로드 합니다."); 
		// 주어진 문구를 출력한다. 
	}
}
```  
정상적으로 출력된 결과를 확인할 수 있다.  

## 후기  

안녕하세요! 새로운 블로그에서 처음 게시한 글이 이 글이네요! Notion을 애용하면서 마크다운 양식에 편해져있었는데, 이전 블로그는 100% 지원하지 않고 테마 오류를 고치기도 어려워 주말부터 열심히 이사를 했답니다!  
이번 상속 내용 중에서 클래스 타입 변환 내용을 처음 글로 읽었을 때는 이해가 잘 되지 않았어요. 정말 처음 보느 내용이었던 거죠. 그런데 자동 타입 변환의 목적과 다형성 개념을 이해하고 나니 왜 사용하는지, 타입 변환이 이루어져야 하는 이유가 무엇인지 파악되면서 개념도 이해가 가기 시작했어요.  
이번은 특히 편한 양식으로 쓰는 블로그라 기분이 좋아서 그런지 평소보다 확인 문제도 많이 풀고 내용도 잘 정리한 것 같네요! 내심 뿌듯해서 다른 사람도 아니고 저 스스로가 제가 쓴 걸 여러번 반복해서 봤습니다. 복습 효과가 있었어요 하하하.  
그 덕분에 기억에서 잊혀질 뻔 한 메소드 재정의 & 접근제한자 내용을 확실히 기억하고 넘어갈 수 있게 되었어요. 다들 추위와 감기 조심하세요.