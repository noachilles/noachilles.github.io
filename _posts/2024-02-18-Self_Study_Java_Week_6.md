---
title: (JAVA) 혼공자바 6주차 - 예외, 기본 API 클래스
date: 2024-02-18 15:30:00 +0900
categories: [Review, JAVA]
tags: [JAVA, 혼공단]
image: /posts/2024-02-18-Self-Study-Java-Week-6/thumbnail.png
---

## 예외 클래스

자바에서는,  
하드웨어 오동작이나 고장으로 발생하는 오류는 에러(Error), 프로그램 자체에서 발생하는 오류는 예외(Exception)라 한다.

컴파일 시 확인할 수 있는 예외는 일반 예외라고 하며, 컴파일러 체크 예외라고도 한다.  
컴파일 시 확인할 수 없고, 프로그램 실행 중 발생하는 예외는 실행 예외라고 하며, 컴파일러 넌 체크 예외라고 한다.

자바에서는 예외를 클래스로 관리하며, 모든 예외는 java.lang.Exception 클래스를 상속받는다.

일반 예외, 실행 예외는 RuntimeException 클래스를 상속하고 있는지 여부에 따라 구분할 수 있다. 위 클래스를 상속한다면 실행 예외, 상속하지 않으면 일반 예외이다.

### 실행 예외

- 컴파일러가 체크하지 않기 때문에 개발자의 경험에 의해 작성해야 한다.

**NullPointerException**

- null 값을 갖는 참조 변수로 객체에 접근하려고 했을 때 발생
- 즉, 가리키는 객체가 없는데 객체를 이용하려고 할 때의 예외

**ArrayIndexOutOfBoundsException**

- 배열을 사용할 때, 인덱스 범위를 초과하면 발생하는 예외
- 이를 방지하기 위해 배열에 값이 들어있는지 확인하는 절차가 필요

**NumberFormatException**

- 문자열로 된 데이터를 숫자로 변경하기 위해서 `Integer.parseInt(String s)`를 사용하거나 `Double.parseDouble(String s)`를 사용할 때, 인자로 들어온 문자열이 숫자로 변환될 수 없는 문자열이면 발생

**ClassCastException**

- 자동 타입 변환된 변수가 아닌데 강제 타입 변환을 시도할 때 발생하는 예외
- `instanceof` 연산자로 타입 변환이 가능한지 확인하고 수행해 방지할 수 있음

**✔️확인 문제**

1.예외에 대한 설명 중 틀린 것은 무엇입니까?  
① 예외는 사용자의 잘못된 조작, 개발자의 잘못된 코딩으로 인한 프로그램 오류를 말한다. (O)  
② RuntimeException의 하위 클래스는 컴파일러가 예외 처리 코드를 체크하지 않는다. (O)  
③ 예외는 클래스로 관리된다. (O)  
④ Exception의 하위 클래스는 모두 일반 예외에 해당한다. (X)  
<span style="color: #f08080">
▶ 모든 예외는 Exception 클래스를 상속 받는다.  
</span>

---

## 예외 처리

- 자바 컴파일러는 일반 예외 발생 가능성이 있는 코드에 대해 개발자가 예외 처리 코드를 작성하도록 함

### try-catch-finally 블록

- `try` 블록에는 예외 발생 가능 코드가 위치

- `catch` 블록에는 예외 발생시 실행할 코드 위치

- `finally` 블록에는 예외 발생과 별개로 항상 실행되는 코드 위치

- 단, `finally` 블록에 있는 코드는 `try` 블록이나 `catch` 블록에서 `return` 을 사용해도 실행됨

### 다중 catch 블록

- 예외 종류에 따라 예외 처리를 다르게 하고 싶을 때 사용

- catch 블록이 여러 개 있어도 단 하나의 catch 블록만 실행됨

- 상위 예외 클래스와 하위 예외 클래스에 대한 catch 블록을 작성한다면, 하위 클래스가 먼저 작성되어야 함  
  ➡️ 그렇지 않을 시 항상 상위 클래스 예외 처리가 실행될 수 있음

**✔️확인 문제**  
1.try-catch-finally 블록에 대한 설명 중 틀린 것은 무엇입니까?  
① try {} 블록에는 예외가 발생할 수 있는 코드를 작성한다. (O)  
② catch {} 블록은 try {} 블록에서 발생한 예외를 처리하는 블록이다. (O)  
③ try {} 블록에서 return문을 사용하면 finally {} 블록은 실행되지 않는다. (X)  
<span style="color:#f08080"> ▶ `try` 블록에서 `return` 문을 사용해도 `finally` 블록은 실행된다.  
</span>
④ catch {} 블록은 예외의 종류별로 여러 개를 작성할 수 있다. (O)

---

### 예외 떠넘기기

- `throws` 키워드를 통해 해당 메소드를 호출한 곳으로 예외를 떠넘길 수 있음  
  Ex) method1에서 method2를 호출한 경우

- `throws` 키워드가 붙은 메소드는 반드시 `try` 블록 내에서 호출되어야 함

- main() 메소드에서도 `throws`를 사용할 수 있으며, 최종적으로 JVM이 예외 처리를 해줌

**✔️확인 문제**  
2.throws에 대한 설명으로 틀린 것은 무엇입니까?

① 생성자나 메소드의 선언 끝 부분에 사용되어 내부에서 발생된 예외를 떠넘긴다. (O)  
② throws 뒤에는 떠넘겨야 할 예외를 쉼표(,)로 구분해서 기술한다. (O)  
③ 모든 예외를 떠넘기기 위해 간단하게 throws Exception으로 작성할 수 있다. (O)  
④ 새로운 예외를 발생시키기 위해 사용된다. (X)  
<span style="color: #f08080">
▶ 발생한 예외를 처리하기 위해 사용된다.  
</span>

3.다음과 같은 메소드가 있을 때 예외를 잘못 처리한 것은 무엇입니까?

```java
public void method1() throws NumberFormatException, ClassNotFoundException { ... }
```

①

```java
try {
    method1();
} catch(Exception e){
}
```

②

```java
void method2() throws Exception{
    method1();
}
```

<span style="color: #f08080"> ▶ `throws` 연산자를 사용하는 코드에 대해 `try` 블록 안에서 호출해야 한다. </span>  
③

```java
try{
    method1();
} catch(Exception e){
} catch(ClassNotFoundException e){
}
```

④

```java
try{
    method1();
} catch(ClassNotFoundException e){
} catch(NumberFormatException e){
}
```

---

## java.lang 패키지

- 자바 프로그램의 기본적인 클래스를 담고 있는 패키지

- [JAVA API Document](https://docs.oracle.com/en/java/javase/index.html) 링크를 통해서 버전별 링크를 클릭해 도큐먼트 페이지로 이동할 수 있음

- 자바 제공 API는 방대하기 때문에 외울 수 없고, 위의 도큐먼트를 잘 쓰는 것이 최선!

### Object 클래스

- 자바의 모든 클래스의 부모 클래스(=최상위 부모 클래스)

**객체 비교**

- 주소가 아닌 내용이 동일한지 비교해 결과를 도출하는 `equals()` 메소드를 재정의해 사용할 수 있음

- String 클래스에서는 이미 재정의된 상태로 사용되고 있음

**객체 해시코드**

- 객체를 식별하는 하나의 정수값

- `hashCode()`가 하는 역할: 객체를 식별할 수 있는 정수 반환

- `hashCode()` 메소드 자체가 비교를 수행하는 메소드는 아님. 다만 `hashCode()` 반환값을 사용하는 `HashSet, HashMap, Hashtable`에서 두 객체가 <span style="color: #f08080">논리적으로 동등</span>
  한지 비교할 수 있음

- 위에서 소개한 프레임워크들에서는 `hashCode()` 리턴값을 우선 살피고, 논리적으로 동등하다는 결과가 나오면 equals()로 두 객체의 내용을 비교함

- `hashCode()` 메소드를 통해 두 객체가 논리적으로 동일한 객체인지 여부를 파악할 수 있음

475p. 예제를 이해해보자!

```java
package sec01.exam03;

public class Member {
	public String id;

	public Member(String id) {
		this.id = id;
	}

	@Override
	public boolean equals(Object obj) {
		if(obj instanceof Member) {
			Member member = (Member) obj;
			if(id.equals(member.id)) {
				return true;
			}
		}
		return false;
	}

	@Override
	public int hashCode() {
		return id.hashCode(); // 교재: 같은 문자열인 경우 동일한 해시코드를 리턴
	}
}

```

위의 주석에 적힌 내용이 어떻게 수행되는지 이해가 가지 않았다.  
`id`는 `String` 변수였기 때문에 앞에서 정리한 API Document로 재정의된 내용을 살펴보고 이해할 수 있었다.

**🌠Java API Document로 String 클래스의 hashCode() 메소드 재정의 확인하기**

![Desktop View](/posts/2024-02-18-Self-Study-Java-Week-6/fig1.png)  
[JAVA API Document](https://docs.oracle.com/en/java/javase/index.html)에 접속 한 후의 화면이다. 환경 변수를 통해 컴퓨터에 설치된 JDK 버전을 확인하고 화면에서 동일한 버전을 선택하면 된다.

![Desktop View](/posts/2024-02-18-Self-Study-Java-Week-6/fig2.png)  
화면 상단 검색창에 `java.lang` 패키지를 검색한다.

![Desktop View](/posts/2024-02-18-Self-Study-Java-Week-6/fig3.png)  
사진 속 링크로 접속한다.

![Desktop View](/posts/2024-02-18-Self-Study-Java-Week-6/fig4.png)  
우측 상단 검색창에 `String`을 검색한다.

![Desktop View](/posts/2024-02-18-Self-Study-Java-Week-6/fig5.png)  
`hashCode()` 메소드 내용을 찾는다.

String 클래스의 hashCode() 메소드 내용을 읽어보면, 각각의 문자열 객체에 속한 각 문자에 대한 연산으로 정수 값을 반환하는 사실을 알 수 있다.  
<span style="color: #f08080">🌟즉, 동일한 문자열을 저장한 String 객체들은 동일한 해시코드 값을 가진다.
</span>

**✔️확인 문제**  
1.Object 클래스에 대한 설명 중 틀린 것은 무엇입니까?

① 모든 자바 클래스의 최상위 부모 클래스이다. (O)  
② Object의 equals() 메소드는 == 연산자와 동일하게 번지를 비교한다. (O)  
③ 동등 비교를 위해 equals()와 hashCode() 메소드를 재정의하는 것이 좋다. (O)  
④ Object의 toString() 메소드는 객체의 필드값을 문자열로 리턴한다. (X)  
<span style="color: #f08080">
▶ Object의 toString() 메소드는 필드값이 아닌 객체의 문자 정보(객체를 문자열로 표현한 정보)를 리턴한다.
</span>

3.student의 학번(studentNum)이 같으면 동등 객체가 될 수 있는 Student 클래스를 작성해보세요.  
<span style="color: #f08080">Student 클래스</span>

```java
package sec01.test03;

public class Student {
	private String studentNum;

	public Student(String studentNum) {
		this.studentNum = studentNum;
	}

	public String getStudentNum() {
		return studentNum;
	}

	// 논리적 동등 확인: hashCode() 오버라이딩
	@Override
	public int hashCode(){
		return studentNum.hashCode();
	}

	// 내용 동일 확인: equals() 오버라이딩
	@Override
	public boolean equals(Object obj) {
		if(obj instanceof Student) {
			Student student = (Student) obj;
			if(studentNum.equals(student.studentNum)) {
				return true;
			}
		}
		return false;
	}
}
```

<span style="color: #f08080">Main() 클래스</span>

```java
package sec01.test03;

import java.util.HashMap;

public class StudentExample {

	public static void main(String[] args) {
		// Student 키로 총점을 저장하는 HashMap 객체 생성
		HashMap<Student, String> hashMap = new HashMap<Student, String>();

		// new Student("1")의 점수 95를 저장
		hashMap.put(new Student("1"), "95");

		// new Student("1)로 점수를 읽어옴
		String score = hashMap.get(new Student("1"));
		System.out.println("1번 학생의 총점: " + score);
	}

}
```

<span style="color: #f08080">실행 결과</span>

```cmd
1번 학생의 총점: 95
```

---

**객체 문자 정보**

- `Object` 클래스의 `toString()` 메소드는 '클래스이름@16진수해시코드'로 구성된 문자 정보를 리턴

- `Date` 클래스는 `toString()` 메소드를 재정의해 현재 시스템의 날짜와 시간 정보를 리턴함

### System 클래스

- JVM에서 실행되므로 자바는 운영체제의 모든 기능을 직접 이용하기에 무리가 있음

- `System` 클래스를 이용해 운영체제의 일부 기능을 이용할 수 있음

- 모든 필드와 메소드는 정적 필드와 정적 메소드로 구성되어 있음

**프로그램 종료**

- `exit()` 메소드는 현재 실행중인 프로세스를 강제 종료시키는 역할을 수행

- `exit()` 메소드는 `int` 자료형의 종료 상태값을 매개값으로 받음

- 정상 종료의 경우에 종료 상태값은 `0`

**현재 시각 읽기**

- `currentTimeMillis()` 메소드와 `nanoTime()` 메소드를 통해 컴퓨터로부터 현재 시각을 읽어 각각 밀리세컨드, 나노세컨드 단위의 `long` 타입 값을 리턴

### Class 클래스

- 클래스와 인터페이스의 메타 데이터(이름, 생성자 정보, 필드 정보, 메소드 정보 등)를 관리하는 클래스

- 다음과 같은 방식으로 `Class` 객체를 얻을 수 있다.

클래스를 이용하는 방법

```java
① Class clazz = 클래스이름.class;
② Class clazz = Class.forName("패키지...클래스이름");
```

객체를 이용하는 방법

```java
③ Class clazz = 참조변수.getClass();
```

위와 같은 방법을 사용하면 특정 클래스의 `Class` 객체를 얻을 수 있다.

📍 Class 객체는 해당 클래스의 파일 경로 정보를 저장하고 있어 다른 리소스 파일의 절대 경로를 불러오는 데 사용할 수 있음

**✔️확인 문제**  
5.Class 객체에 대한 설명 중 틀린 것은 무엇입니까?

① Class.forName() 메소드 또는 객체의 getClass() 메소드로 얻을 수 있다. (O)  
② 클래스의 생성자, 필드, 메소드에 대한 정보를 알아낼 수 있다. (O)  
③ 클래스 파일을 기준으로 상대 경로의 리소스의 정보를 얻을 수 있다. (O)  
④ 클래스.class로 Class 객체를 얻을 수 없다. (X)  
<span style="color:#f08080">
▶ `Class clazz = 클래스.class` 를 작성해 Class 객체를 얻을 수 있다.

---

### String 클래스

- 문자열을 생성하는 방법과 추출, 비교, 찾기, 분리, 변환 등을 제공하는 클래스

- 자바의 문자열은 이 String 클래스의 인스턴스로서 관리됨

- 파일 내용을 읽거나, 네트워크를 통해 받은 데이터는 보통 byte[] 배열이므로 이것을 문자열로 변환하기 위한 생성자가 존재함

**✔️확인 문제**  
6.다음에 주어진 바이트 배열을 문자열로 변환해보세요.

<span style="color:#f08080">Main() 클래스
</span>

```java
package sec01.test06;

public class BytesToStringExample {
	public static void main(String[] args) {
		byte[] bytes = {73, 32, 108, 111, 118, 32, 121, 111, 117};
        // 답변, byte 배열을 매개변수로 하는 생성자를 이용했다.
		String str = new String(bytes);
		System.out.println(str);
	}
}
```

<span style="color:#f08080">실행 결과</span>

```cmd
I lov you
```

로맨틱하다...

---

### Wrapper(포장) 클래스

- 포장 객체(기본 타입 값을 갖는 객체)를 생성하는 클래스

- 포장 객체가 포장하고 있는 기본 타입 값은 외부에서 변경할 수 없음

- 만약 변경을 원한다면 새로운 포장 객체를 생성해야 함

- 기본 타입에 대응되는 클래스들이 있고, 각 타입의 첫글자를 대문자로 바꾼 이름을 가짐

**박싱**

- 기본 타입의 값을 포장 객체로 만드는 과정

- `타입대응클래스 obj = new 타입대응클래스(값 or 문자열)` 방식으로 만들 수 있음

- `타입대응클래스 obj = 타입대응클래스.valueOf(값 or 문자열)` 방식으로 만들 수도 있음

**언박싱**

- 포장 객체에서 기본 타입의 값을 얻어내는 과정

- `기본타입 변수이름 = obj.기본타입Value()` 방식으로 기본 타입의 값을 다시 얻어낼 수 있음

**자동 박싱과 언박싱**

- 자동 박싱: <span style="color:#f08080">
  포장 클래스 타입에</span>
  기본값이 대입될 경우 발생

- 자동 언박싱: <span style="color:#f08080">
  기본 타입에</span>
  포장 객체가 대입되는 경우 Or 포장 객체와 기본 타입간 연산이 있을 때 발생

```java
// 자동 박싱: 포장 객체에 기본값이 대입됨
Integer obj = 100;
System.out.println("value: " + obj.intValue());

// 자동 언박싱: 기본 타입에 포장 객체 대입됨
int value = obj;
System.out.println("value: " + value);

// 자동 언박싱: Integer 객체와 int 타입 연산시
int result = obj + 100;
System.out.println("result: " + result);
```

- 포장 객체는 내부 값 비교를 위해 비교연산자대신 equals() 사용을 추천

### Math 클래스

- 수학 계산에 사용할 수 있는 메소드 제공

- 모두 정적 메소드로, Math 클래스로 바로 사용 가능

## java.util 패키지

- 프로그램 개발에서 자주 사용되는 자료구조뿐 아니라, 날짜 정보를 제공해주는 API

### Date 클래스

- 날짜를 표현하는 클래스

- 객체 간에 날짜 정보를 주고 받을 때 매개 변수나 리턴 타입으로 주로 사용됨

- **toString()** 메소드는 영문으로 된 날짜를 리턴하므로 원하는 날짜 형식이 있다면 **SimpleDateFormat** 클래스와 함께 사용하는 것이 좋음

### Calendar 클래스

- 달력을 표현한 **추상 클래스**

## 기본 미션

**439p. 확인문제 2번 풀이하기**  
2.AnonymousExample 클래스 실행결과를 바탕으롱 Vehicle 인터페이스의 익명 구현 객체를 이용해 필드, 로컬 변수의 초기값과 메소드의 매개값을 대입해보세요.

<span style="color:f08080">인터페이스</span>

```java
package sec02.test02;

public interface Vehicle {
	public void run();
}
```

<span style="color:f08080">익명 구현 클래스와 객체 생성</span>

```java
package sec02.test02;

public class Anonymous {
    // 1. 클래스 필드로 익명 구현 객체를 생성함
	Vehicle field = new Vehicle() {
		@Override
		public void run() {
			System.out.println("자전거가 달립니다.");
		}
	};

    // 2. 메소드의 로컬 필드로 익명 구현 객체를 생성함
	void method1() {
		Vehicle localVar = new Vehicle() {
			@Override
			public void run() {
				System.out.println("승용차가 달립니다.");
			}
		};
		localVar.run();
	}

    // 3. 인터페이스인 Vehicle 타입을 매개변수로 하는 메소드 정의
	void method2(Vehicle v) {
		v.run();
	}
}
```

<span style="color:f08080">Main()메소드가 있는 클래스</span>

```java
package sec02.test02;

public class AnonymousExample {
	public static void main(String[] args) {
		Anonymous anony = new Anonymous();

        // 1. 필드로 생성된 익명 구현 객체에서 재정의된 run() 메소드 실행
		anony.field.run();

        // 2. method1()의 로컬 필드로 생성된 익명 구현 객체에서 재정의된 run() 메소드 실행
		anony.method1();

        // 3. method2()의 매개변수로 익명 구현 객체 생성
		anony.method2(
			new Vehicle() {
				@Override
				public void run(){
					System.out.println("트럭이 달립니다.");
				}
			}
		);

	}
}
```

---

## 선택 미션

**java.lang의 주요 클래스들의 이름과 용도를 정리하기**

`Object`:

- 모든 클래스의 최상단 부모 클래스로, `equals()` `hashCode()`등 객체를 비교할 수 있는 메소드와 `toString()`등 유용한 객체 정보를 출력할 수 있는 메소드를 제공한다.

`System`:

- JVM 위에서 실행되는 특징을 가지는 자바에서 OS 기능을 사용하기 위한 클래스로, `System.out.println`이라는 구문으로 가장 많이 마주친 클래스이다.
- `exit()` 메소드 등 OS 기능을 활용할 수 있도록 한다.

`Class`:

- 객체 정보가 있는 메타데이터를 저장해둔 클래스이다.
- 클래스의 정보와 이외 데이터들의 경로를 얻는 데 주로 사용된다.

`String`:

- 모든 문자열 인스턴스의 바탕이 되는 클래스이다.
- 문자열을 조작하는 다양한 종류의 메소드가 정의되어 있다. 또한 생성자 형태 역시 다양해 파일로부터 문자열을 읽어오는 역할을 한다.

`Wrapping`:

- 기본값을 객체로 이용하기 위한 '포장 객체'를 생성하는 클래스를 뜻한다. 기본 타입값(int, boolean, double, ...)에서 첫글자만 대문자로 바뀐 형태를 가지고 있으며 포장된 객체는 외부에서 값을 수정할 수 없게끔 설정되어 있다.

## 후기

이번주 후기 + 전체 후기로 회고 블로그에서 만나요^\_\_^
