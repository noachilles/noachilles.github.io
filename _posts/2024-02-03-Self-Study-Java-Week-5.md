---
title: (JAVA) 혼공자바 5주차 - 인터페이스, 익명 객체
author: noa
date: 2024-02-03 15:30:00 +0900
categories: [Study, JAVA]
tags: [JAVA, 혼공단]
image: /posts/2024-02-03-Self-Study-Java-Week-5/thumbnail.PNG
---

![Desktop View](/posts/2024-02-03-Self-Study-Java-Week-5/thumbnail.PNG)  

## 인터페이스(Interface)
* 참조중인 객체로부터 소스코드에서 호출한 메소드를 실행/반환해주는 역할을 수행하는 타입(소스코드에서 필요한 객체와 호환된다.)  

* 각 객체에 따라 실행 내용과 반환값을 다르게 할 수 있어 활용도가 높음  

* 접근제한자 이후에 ```class``` 대신 ```interface```를 작성해 선언  

* 인터페이스 안에는 상수 필드와 추상 메소드(abstract)만을 정의할 수 있음  

### 인터페이스 선언  

* 상수 필드는 ```static final``` 이후 타입으로 정의되어 왔으나, 인터페이스가 가질 수 있는 필드는 상수 필드뿐이므로 ```static final```을 생략해도 선언 가능  

* 추상 메소드 역시 접근 제한자와 ```abstract```를 생략해도 선언 가능(*기본 접근제한자는 public)  

인터페이스는 추상 클래스와 같이, 직접 객체를 생성할 수 없다.  
인터페이스가 호출할 수 있도록, 인터페이스의 추상 메소드를 구현해 포함하고 있는 객체를 구현(Implement) 객체라고 하며, 구현 객체를 생성하는 클래스를 구현(Implement) 클래스라고 한다.  

```java
public class Audio implements RemoteControl{

}
```
위 코드에서는 RemoteControl이라는 인터페이스로부터 Audio라는 구현클래스를 선언하였다.  
* 구현클래스에서는 인터페이스의 추상 메소드를 실체화 해야함  

* 인터페이스로 구현 객체를 사용하려면 인터페이스 변수를 선언하고, 구현 객체를 대입해야 함  

* 인터페이스 변수를 선언해 사용하지 않으면 인터페이스 자체를 사용하지 않은 것이 됨(그냥 구현 객체를 사용한 것)  

* 다중 인터페이스 구현: 상속과 달리, JAVA에서는 여러 개의 인터페이스를 구현할 수 있음  

**✔️확인 문제**  
1.인터페이스에 대한 설명입니다. 맞는 것에 O표, 틀린 것에 X표 하세요.  
① 인터페이스는 객체 사용 방법을 정의해놓은 타입이다. (O)  
② new 연산자를 이용해서 인터페이스 객체를 만들 수 있다. (X)  
<span style="color: #f08080">
▶ 인터페이스로는 객체를 만들 수 없다. 별도 구현 클래스를 정의해주어야 한다.
</span>  
③ 인터페이스는 상수 필드와 추상 메소드를 갖는다. (O)  
④ 구현 클래스는 implements 키워드로 인터페이스를 기술해야 한다. (O)

---

### 인터페이스 사용  

1. 인터페이스가 필드 타입으로 사용되면, 필드에 구현 객체 대입이 가능함

2. 인터페이스가 생성자 매개 변수 타입으로 사용되면, new 연산자로 객체를 생성할 때 구현 객체를 생성자 매개값으로 대입 가능함  

3. 인터페이스가 로컬 변수 타입으로 사용되면, 변수에 구현 객체 대입이 가능함  

4. 인터페이스가 메소드의 매개 변수 타입으로 사용되면, 메소드 호출 시 구현 객체를 매개값으로 대입이 가능함  

**✔️확인 문제**  
2. 인터페이스 사용에 대한 설명입니다. 맞는 것에 O표, 틀린 것에 X표 하세요.  
① 클래스를 선언할 때 인터페이스 타입의 필드를 선언할 수 있다. (O)  
② 생성자의 매개 타입이 인터페이스일 경우, 매개값으로 구현 객체를 대입한다. (O)  
③ 인터페이스 타입의 로컬 변수는 선언할 수 없다. (X)  
<span style="color: #f08080">
▶ 인터페이스 타입의 로컬 변수를 선언할 수 있다.
</span>  
④ 메소드의 매개 타입이 인터페이스일 경우, 매개값으로 구현 객체를 대입한다. (O)  

---

## 타입 변환과 다형성  

* 앞서 <인터페이스 사용>에서 언급한 내용과 유사하게 인터페이스 타입 변수에 구현 객체를 대입할 수 있음(자동 타입 변환)  

* 대입한 구현 객체를 변경하는 것으로 프로그램 실행 결과를 변경할 수 있음(=필드 다형성)  

* 상속 내용과 유사하게 필드의 다형성을 활용할 수 있음  
(부모 클래스 = 인터페이스), (자식 클래스 = 구현 클래스)  

* 메소드의 매개변수 타입이 인터페이스일 경우, 구현 클래스 객체 사용 가능(=매개 변수 다형성)  

**✔️확인 문제**  
2. 다형성에 대한 설명입니다. 맞는 것에 O표, 틀린 것에 X표 하세요.  

① 다형성을 구현하기 위한 조건은 메소드 재정의와 타입 변환이다. (O)  
<span style="color: #008080">
▶</span>
 다양한 객체 종류를 생성 및 활용할 수 있는 다형성을 구현하기 위해서는 
<span style="background-color: #008080">
인터페이스의 추상 메소드를 재정의하는 메소드 재정의(Overriding)
</span>와 
<span style="background-color: #008080">
구현 객체를 인터페이스 타입으로 변환하는 타입 변환
</span>이 필수적이다.  
② 클래스 상속과 인터페이스는 모두 메소드 재정의와 타입 변환 기능이 제공되므로, 어떤 방법을 사용하든 다형성 구현이 가능하다. (O)  
③ 매개 변수의 타입이 클래스라면 해당 클래스로 생성된 객체만 대입이 가능하다. (X)  
<span style="color: #f08080">
▶ 자식 클래스 객체 역시 대입 가능하다.
</span>  
④ 매개 변수의 타입이 인터페이스라면 모든 구현 객체가 대입이 가능하다. (O)  

---

### 강제 타입 변환  

* 자동 타입 변환될 경우, 인터페이스에 선언된 메소드만 사용 가능  

* 자동 타입 변환의 한계를 보완할 수 있는 방법이 강제 타입 변환  

* 구현 클래스 객체가 대입된 인터페이스 변수를 구현 클래스 변수로 변환하는 게 인터페이스와 구현 클래스 간 강제 타입 변환  

* 상속 때와 마찬가지로 ```instanceof``` 연산자를 사용해 객체 타입을 확인할 수 있음  

🌟 한 번 더 짚고 넘어가자면, 
<span style="color: #f08080">
자식</span>이 
<span style="color: #f08080">
부모 타입으로</span> 
타입 변환 되는 것이 자동 타입 변환이다.  

**✔️확인 문제**  
1.인터페이스 타입 변환에 대한 설명입니다. 맞는 것에 O표, 틀린 것에 X표 하세요.  

① 구현 객체는 인터페이스 타입으로 자동 변환된다. (O)  
② 부모 클래스가 인터페이스를 구현하면 자식 클래스로부터 생성된 객체는 인터페이스 타입으로 자동 변환할 수 없다. (X)  
<span style="color: #f08080">
▶ 자식 객체는 부모 클래스 타입으로 자동 타입 변환되며, 구현 객체는 인터페이스로 자동 타입 변환되므로 가능하다.
</span>  
③ 인터페이스 변수에 대입된 객체를 원래 구현 클래스 타입으로 변환하는 것을 강제 타입 변환이라고 한다. (O)  
④ 메소드의 매개변수 타입이 인터페이스이면 매개값으로 모든 구현 객체를 대입하면 자동 타입 변환이 된다. (O)  

---

### 인터페이스 상속  

* 인터페이스는 상속이 가능하다. 특히 다중 상속이 가능하다!(클래스 상속과 가장 다른 점)  

* 하위 인터페이스는 상위 인터페이스의 모든 추상 메소드에 대한 실체 메소드를 가지고 있어야 한다.  
=> 따라서 구현 클래스로부터 객체를 먼저 생성해야 한다.

* 상속 받은 모든 인터페이스로의 자동 타입 변환이 가능하다.  

* 하위 인터페이스는 실체 메소드를 가져야 한다. (인터페이스 메소드는 모두 추상 메소드로 선언되는데 인터페이스를 상속하면 그 특성이 유지되지 않아 왜 해야 하는지 모르겠다.)  

**✔️확인 문제**  
3. (문제는 교재 참고)  
<span style="color: #f08080">
DaoExample
</span>
```java
package sec02.test03;

public class DaoExample {

	public static void dbWork(DataAccessObject dao) {
		// DataAccessObject가 가져야하는 추상 메소드: select, insert, update, delete
		dao.select();
		dao.insert();
		dao.update();
		dao.delete();
	}
	
	public static void main(String[] args) {
		// TODO Auto-generated method stub
		dbWork(new OracleDao()); // DataAccessObject(인터페이스)에 OracleDao 객체를 대입
		dbWork(new MySqlDao()); // DataAccessObject에 MySqlDao 객체를 대입

	}

}
```  
<span style="color: #f08080">
DataAccessObject
</span>  

```java
package sec02.test03;

public interface DataAccessObject {
	public abstract void select();
	public abstract void insert();
	public abstract void update();
	public abstract void delete();
}
```  

<span style="color: #f08080">
OracleDao
</span>  

```java
package sec02.test03;

public class OracleDao implements DataAccessObject{
	@Override 
	// implements 로 DataAccessObject 구현 클래스임을 밝힘
	// 메소드 재정의  
	public void select() {
		System.out.println("Oracle DB에서 검색");
	}
	
	public void insert() {
		System.out.println("Oracle DB에 삽입");
	}
	
	public void update() {
		System.out.println("Oracle DB를 수정");
	}
	
	public void delete() {
		System.out.println("Oracle DB에서 삭제");
	}
}
```

<span style="color: #f08080">
MySqlDao
</span>  

```java
package sec02.test03;

public class MySqlDao implements DataAccessObject{
	@Override 
	// implements 로 DataAccessObject 구현 클래스임을 밝힘
	// 메소드 재정의  
	public void select() {
		System.out.println("MySql DB에서 검색");
	}
	
	public void insert() {
		System.out.println("MySql DB에 삽입");
	}
	
	public void update() {
		System.out.println("MySql DB를 수정");
	}
	
	public void delete() {
		System.out.println("MySql DB에서 삭제");
	}
}
```

<span style="color: #f08080">
실행 결과
</span>  

![Desktop View](/posts/2024-02-03-Self-Study-Java-Week-5/fig1.png)  

---

## 중첩 클래스

* 중첩 클래스: 클래스 내부에 선언한 클래스  
목적: 두 클래스 멤버간 접근이 쉬움

* 클래스 멤버로 선언되면 ➡️ 멤버 클래스  
메소드 내부에 선언되면 ➡️ 로컬 클래스  

* **<span style="background-color: #ffdce0; color: black">인스턴스 멤버 클래스</span>**: ```static``` 키워드 없이 중첩 선언된 클래스로, 정적 멤버 선언 불가  

* **<span style="background-color: #fff5b1; color: black">정적 멤버 클래스</span>**: ```static``` 키워드로 중첩 선언된 클래스로, 모든 종류 멤버 선언 가능

* <span style="background-color: #ffdce0; color: black">인스턴스 멤버 클래스</span>
는 바깥 클래스의 ```static``` 필드와 메소드에서 객체를 생성할 수 없음.   
<span style="background-color: #fff5b1; color: black">
정적 멤버</span>
는 바깥 클래스의 모든 필드&메소드에서 객체 생성 가능  

* <span style="background-color: #ffdce0; color: black">인스턴스 멤버 클래스</span>
내부에서는 바깥 클래스 모든 멤버에 접근 가능   
<span style="background-color: #fff5b1; color: black">
정적 멤버</span> 
클래스 내부에서는 바깥 클래스 정적 멤버에만 접근 가능

* <span style="background-color: #ffdce0; color: black">인스턴스 멤버 클래스</span> 
객체는 바깥 클래스 객체가 있어야 생성 가능  
<span style="background-color: #fff5b1; color: black">
정적 멤버</span> 
객체는 바깥 클래스 객체가 없어도 생성 가능

```java
// A 내부에 인스턴스 멤버 클래스 B가 있는 상황
A a = new A(); 

A.B b = a.new B();
b.field1 = 3;
b.method1();
```  
**✔️확인 문제**  
1.중첩 멤버 클래스에 대한 설명입니다. 맞는 것에 O표, 틀린 것에 X표 하세요.  

① 인스턴스 멤버 클래스는 바깥 클래스의 객체가 있어야 사용될 수 있다. (O)  
② 정적 멤버 클래스는 바깥 클래스의 객체가 없어도 사용될 수 있다. (O)  
③ 인스턴스 멤버 클래스 내부에는 바깥 클래스의 필드와 메소드를 사용할 수 있다. (O)  
④ 정적 멤버 클래스 내부에는 바깥 클래스의 인스턴스 필드를 사용할 수 있다. (X)  
<span style="color: #f08080">
▶ 정적 멤버 클래스 내부에는 바깥 클래스의 정적 필드만 사용 가능하다. 
</span>

---


* **로컬 클래스**: 접근 제한자와 ```static``` 사용 불가. 오로지 메소드 내부에서만 사용됨 정적 필드와 메소드 선언 불가

* 스레드 및 로컬 클래스 객체 실행 문제로 로컬 클래스 내부 매개 변수와 로컬 변수 값은 final 속성을 가짐  

* 중첩 클래스에서 ```바깥클래스.this.멤버``` 를 사용하면 바깥 클래스 참조가 가능  

**✔️확인 문제**  
2. 로컬 클래스에 대한 설명으로 틀린 것은 무엇입니까?  

① 로컬 클래스는 생성자 또는 메소드 내부에 선언된 클래스를 말한다. (O)  
② 로컬 클래스도 필드와 생성자를 가질 수 있다. (O)  
③ 로컬 클래스는 ```static``` 키워드를 이용해 정적 클래스로 만들 수 있다. (X)  
<span style="color: #f08080">
▶ 로컬 클래스 선언에서는 접근 제한자, ```static``` 키워드를 사용할 수 없다.
</span>  
④ ```final``` 특성을 가진 매개 변수나 로컬 변수만 로컬 클래스 내부에서 사용할 수 있다. (O)  

---

### 중첩 인터페이스  

* 중첩 인터페이스: 클래스 내부에 선언한 인터페이스  
해당 클래스와 긴밀한 관계를 맺는 구현 클래스 만들 수 있음

* 인스턴스&정적 멤버 인터페이스 모두 선언 가능  

* 인스턴스 멤버 인터페이스: 바깥 클래스 객체가 있어야 사용 가능  

* 정적 멤버 인터페이스: 바깥 클래스 객체 없이도 사용 가능  
🌟 주로 정적 멤버 인터페이스를 사용한다.  

아래 예제를 통해 중첩 인터페이스의 선언 위치는 사용과 관계 없음을 알 수 있다.  
외부에서 구현할 때는 ```외부클래스.인터페이스``` 양식을 사용한다.

```java
public class Button {
	OnClickListener listener;
	
	void setOnClickListener(OnClickListener listener){
		this.listener = listener;
	}
	
	void touch() {
		listener.onClick();
	}
	
	static interface OnClickListener{
		void onClick();
	}
}
```  

## 익명 자식 객체  

* 간단하게 만들 수 있는 일회용 자식 객체

* 생성 방법:

```
부모클래스 변수 = new 부모클래스(){ ... };
```  

* 새롭게 필드, 메소드 생성 가능하고 Overriding 가능  

* 새로 정의된 필드, 메소드는 익명 자식 객체 내부에서만 사용됨  

* <span style="color: #f08080">생성자는 선언할 수 없음</span>  

```java
anony.method2(
    new Person() {
        void study() {
            System.out.println("공부합니다.");
        }
        @Override
        void wake() {
            System.out.println("8시에 일어납니다.");
            study();
        }
    }

);
```  
위와 같이 매개변수에 직접 대입할 수도 있다. (매개변수 타입은 부모클래스)  

**✔️확인 문제**
1.AnonymousExample 클래스 실행결과로 Worker 클래스 익명 자식 객체 이용해 필드, 로컬 변수의 초기값과 메소드 매개값을 대입해보세요. 

<span style="color:#f08080">
부모 클래스 (인터페이스로 오타 있음!) 
</span>

```java
package sec02.test1;
package sec02.test1;

public class Worker {
	public void start() {
		System.out.println("쉬고 있습니다.");
	}
}
```
<span style="color:#f08080">
익명 자식 클래스(오타 있음)
</span>

```java
package sec02.test1;

public class Anonymous {
	Worker field = new Worker() {
		@Override
		public void start() {
			System.out.println("디자인을 합니다.");
		}
	};
	
	void method1() {
		Worker localVar = new Worker() {
			@Override
			public void start() {
				System.out.println("개발을 합니다.");
			}
		};
		localVar.start();
	}
	
	void method2(Worker worker) {
		worker.start();
	}
}
```  

<span style="color:#f08080">
Main 클래스 
</span>

```java
package sec02.test1;

public class AnonymousExample {
	public static void main(String args[]) {
		Anonymous anony = new Anonymous();
		anony.field.start(); // 디자인을 합니다.
		anony.method1(); // 개발을 합니다.
		anony.method2(
			new Worker() {
				@Override
				public void start() {
					System.out.println("테스트를 합니다.");
				}
			}
		); // 테스트를 합니다.
	}
}
```

---

### 익명 구현 객체 생성  

* 익명 자식 객체와 같이 사용하되, 
<span style="color:#f08080">
인터페이스의 모든 추상 클래스를 재정의해야 한다. 
</span>  

* 생성 방법:  
```
인터페이스 = new 인터페이스(){ ... };
```  

🌟 로컬 클래스와 마찬가지로 스레드 및 생성 이슈로 익명 객체 내부에서 사용할 때 매개 변수와 로컬 변수는 final 특성을 갖는다.  

## 기본 미션  

**클래스를 선언할 때 인터페이스는 어떻게 선언될 수 있는지**  

1. 접근제한자 이후에 ```class``` 대신 ```interface```를 작성해 선언할 수 있다. 

2. 인터페이스 안에는 상수 필드와 추상 메소드(abstract)만을 정의할 수 있다.  

3. 인터페이스는 객체를 가질 수 없고, 내부 추상 메소드를 재정의할 수 있는 구현 클래스를 가질 수 있다. 

4. 특정 인터페이스에 대한 구현 클래스는  ```class 이름 implements 인터페이스``` 양식을 통해 선언할 수 있다.

## 선택 미션  

Car 클래스 내부에 Tire와 Engine 멤버 클래스가 다음과 같이 선언되어 있을 때,

```java
package sec01.test03;

public class Car {
	class Tire{}
	static class Engine{}
}
```  
다음과 같이 멤버 클래스 객체를 외부에서 생성할 수 있다.  
```java
package sec01.test03;

public class NestedClassExample {

	public static void main(String[] args) {
		Car myCar = new Car();
		
        // 인스턴스 멤버 클래스는 바깥 클래스 객체 필요O
		Car.Tire tire = myCar.new Tire();
		
        // 정적 멤버 클래스는 바깥 클래스 객체 필요X
		Car.Engine engine = new Car.Engine();
	}

}
```