---
title: (JAVA) 혼공자바 6주차 - 인터페이스, 익명 객체
date: 2024-02-08 15:30:00 +0900
categories: [Study, JAVA]
tags: [JAVA, 혼공단]
image: /posts/2024-02-08-Self-Study-Java-Week-6/thumbnail.PNG
published: False
---

## 예외 클래스  

자바에서는,  
하드웨어 오동작이나 고장으로 발생하는 오류는 에러(Error), 프로그램 자체에서 발생하는 오류는 예외(Exception)라 한다.  

컴파일 시 확인할 수 있는 예외는 일반 예외라고 하며, 컴파일러 체크 예외라고도 한다.  
컴파일 시 확인할 수 없고, 프로그램 실행 중 발생하는 예외는 실행 예외라고 하며, 컴파일러 넌 체크 예외라고 한다.  

자바에서는 예외를 클래스로 관리하며, 모든 예외는 java.lang.Exception 클래스를 상속받는다.  

일반 예외, 실행 예외는 RuntimeException 클래스를 상속하고 있는지 여부에 따라 구분할 수 있다. 위 클래스를 상속한다면 실행 예외, 상속하지 않으면 일반 예외이다.  

### 실행 예외  

* 컴파일러가 체크하지 않기 때문에 개발자의 경험에 의해 작성해야 한다.  

**NullPointerException**  

* null 값을 갖는 참조 변수로 객체에 접근하려고 했을 때 발생  
* 즉, 가리키는 객체가 없는데 객체를 이용하려고 할 때의 예외  

**ArrayIndexOutOfBoundsException**  

* 배열을 사용할 때, 인덱스 범위를 초과하면 발생하는 예외   
* 이를 방지하기 위해 배열에 값이 들어있는지 확인하는 절차가 필요  

**NumberFormatException**  

* 문자열로 된 데이터를 숫자로 변경하기 위해서 ```Integer.parseInt(String s)```를 사용하거나 ```Double.parseDouble(String s)```를 사용할 때, 인자로 들어온 문자열이 숫자로 변환될 수 없는 문자열이면 발생  

**ClassCastException**  

* 자동 타입 변환된 변수가 아닌데 강제 타입 변환을 시도할 때 발생하는 예외  
* ```instanceof``` 연산자로 타입 변환이 가능한지 확인하고 수행해 방지할 수 있음  

**✔️확인 문제**  

1.예외에 대한 설명 중 틀린 것은 무엇입니까?  
① 예외는 사용자의 잘못된 조작, 개발자의 잘못된 코딩으로 인한 프로그램 오류를 말한다. (O)  
② RuntimeException의 하위 클래스는 컴파일러가 예외 처리 코드를 체크하지 않는다. (O)  
③ 예외는 클래스로 관리된다. (O)  
④ Exception의 하위 클래스는 모두 일반 예외에 해당한다. (X)  
<span style="color: #f08080">
▶ 모든 예외는 Exception 클래스를 상속 받는다.  
</span>  

## 예외 처리  

* 자바 컴파일러는 일반 예외 발생 가능성이 있는 코드에 대해 개발자가 예외 처리 코드를 작성하도록 함  

### try-catch-finally 블록  

* ```try``` 블록에는 예외 발생 가능 코드가 위치  

* ```catch``` 블록에는 예외 발생시 실행할 코드 위치  

* ```finally``` 블록에는 예외 발생과 별개로 항상 실행되는 코드 위치  

* 단, ```finally``` 블록에 있는 코드는 ```try``` 블록이나 ```catch``` 블록에서 ```return``` 을 사용해도 실행됨  

### 다중 catch 블록  

* 예외 종류에 따라 예외 처리를 다르게 하고 싶을 때 사용  

* catch 블록이 여러 개 있어도 단 하나의 catch 블록만 실행됨  

* 상위 예외 클래스와 하위 예외 클래스에 대한 catch 블록을 작성한다면, 하위 클래스가 먼저 작성되어야 함  
➡️ 그렇지 않을 시 항상 상위 클래스 예외 처리가 실행될 수 있음  

**✔️확인 문제**  

1.try-catch-finally 블록에 대한 설명 중 틀린 것은 무엇입니까?  
① try {} 블록에는 예외가 발생할 수 있는 코드를 작성한다. (O)  
② catch {} 블록은 try {} 블록에서 발생한 예외를 처리하는 블록이다. (O)  
③ try {} 블록에서 return문을 사용하면 finally {} 블록은 실행되지 않는다. (X)  
<span style="color:#f08080"> ▶ ```try``` 블록에서 ```return``` 문을 사용해도 ```finally``` 블록은 실행된다.  
</span>
④ catch {} 블록은 예외의 종류별로 여러 개를 작성할 수 있다. (O)  


### 예외 떠넘기기  

* ```throws``` 키워드를 통해 해당 메소드를 호출한 곳으로 예외를 떠넘길 수 있음  
Ex) method1에서 method2를 호출한 경우  

* ```throws``` 키워드가 붙은 메소드는 반드시 ```try``` 블록 내에서 호출되어야 함  

* main() 메소드에서도 ```throws```를 사용할 수 있으며, 최종적으로 JVM이 예외 처리를 해줌  

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
<span style="color: #f08080"> ▶ ```throws``` 연산자를 사용하는 코드에 대해 ```try``` 블록 안에서 호출해야 한다. </span>  
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
