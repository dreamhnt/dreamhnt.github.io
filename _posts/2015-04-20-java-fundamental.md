---
layout: post
title: "자바 강좌 - JAVA Fundamental #1 "
description: "자바의 기본 문법과 개념을 학습한다."
comments: true
category: Study
tags: [java, language]
---


##Data Type

###변수(variable)

- 데이터의 저장과 참조를 위해 할당된 메모리 공간
- 단 하나의 값을 저장할 수 있는 공간

모든 변수에는 타입(Type 또는 형形)이 있으며, 변수의 타입 따라 변수에 저장할 수 있는 값의 종류와 범위가 달라진다.
변수를 선언할 때 저장하고자 하는 값을 고려하여 가장 알맞은 타입을 선택하면 된다. 변수의 타입은 크게 기본형(Primitive Type)과 참조형(Reference Type), 2가지로 나눌 수 있는데, **기본형 변수는 실제 값(Data)을 저장하는 반면에, 참조형 변수는 어떤 값이 저장되어 있는 주소를 값으로 갖는다.**
자바는 C언어와는 달리 String을 제외한 참조형 변수간의 연산을 할 수 없으므로 실제 연산에 사용되는 것은 모두 기본형 변수이다.

####기본자료형

![datatype]({{ "/assets/img/posts/java_dataType.PNG" }})

1. 문자 자료형
  - 문자 하나를 2byte로 하는 **유니코드** 기반 표현
  - 유니코드는 전 세계의 문자를 표현 가능

2. 정수 자료형
  - 정수를 표현하는데 사용하며 byte 크기에 따라 분류됨
  - CPU는 int형 데이터의 크기만 연산 가능
  - byte / short 의 필요성 : 연산보다 데이터의 양이 중요시되는 상황(mp3, 동영상 등등)

3. 실수 자료형
  - float는 소수점 이하 6자리, double는 15자리 정밀도
  - 필요한 정밀도를 바탕으로 자료형을 결정하는데 일반적으로 double을 선호함

4. 논리 자료형
  - true, false중 하나만 저장할 수 있으며 기본값은 false이다.

###상수(Literal)

자바 언어가 처리하는 실제 데이터를 리터럴이라고 한다.

- 자료형을 기반으로 메모리 공간에 저장되며 이름이 없음
- 기본적으로 모든 정수형 상수는 int 형으로, 실수형 상수는 double형으로 표현 및 저장됨

`long num = 1111111111111` 정수형 상수는 기본적으로 int로 저장되기 때문에 위와 같은 코드는 오류를 뿜어낸다. 그렇기 때문에 `long num = 1111111111111L` 이처럼 뒤에 **L** 을 붙여서 long형 리터럴임을 선언해줘야한다.
변수에 명시적인 데이터 타입을 선언해 주었어도 연산시에는 자동적으로 기본 형변환이 되기 때문에 주의해야 한다. 때문에 변수와 리터럴의 데이터 타입을 맞추어서 **형변환(Type Casting)** 을 해주어야한다.

###자동 형 변환 규칙(Implicit Conversion)

![implicit_conversion]({{ "/assets/img/posts/java_implicit_conversion.PNG" }})

##연산자(Operator)

####산술, 대입 연산자

![Operator]({{ "/assets/img/posts/java_op1.PNG" }})

####복합대입연산자

![Operator]({{ "/assets/img/posts/java_op2.PNG" }})

####비교(관계)연산자

![Operator]({{ "/assets/img/posts/java_op3.PNG" }})

####논리연산자

![Operator]({{ "/assets/img/posts/java_op4.PNG" }})

####증감연산자

![Operator]({{ "/assets/img/posts/java_op5.PNG" }})

####비트연산자

![Operator]({{ "/assets/img/posts/java_op6.PNG" }})

####비트시프트연산자

![Operator]({{ "/assets/img/posts/java_op7.PNG" }})

##문(Statement)

###조건문

####if 문

```
if ( 조건식 ) {
	실행문장;
} else if ( 조건식 ) {
	실행문장;
} else {
	실행문장;
}
```

####switch 문

```
switch ( 수식(값) ) {   //수식 값은 반드시 정수 혹은 문자
    case 값1:
        실행문장;
        break;
    case 값2:
        실행문장;
        break;
    default:
        실행문장;
        break;
}
```

###반복문

####for 문

```
for ( 초기식; 조건식; 증감식 ) {
	실행문장;
}
```

*example*

```java
for(int i=0; i < 10; i++) {
	System.out.println("Loop");
}
for(int i=0, j=7; i < j; i++, j--) {
	System.out.println("Loop");
}
```

####for-each 문

```
for ( Object : Objects ) {
	실행문장;
};
```

*example*

```java
String[] arr = { "apple", "pie", "banana" };
for( String obj : arr ) {
	System.out.println(obj);
}
```

####while 문

```
while ( 조건식 ) {
	실행문장;
}
```

####do-while 문

```
do {
	실행문장;
} while ( 조건식 );
```

##Object Oriented Programming(OOP)

Object의 사전적 의미는 사물, 물체, 대상을 뜻하는데 OOP에서는 표현하려는 모든 것들을 Object라 할 수 있다.  **객체 지향 프로그래밍(OOP)** 이란 객체 중심의 프로그래밍을 말한다.
*"나는 과일장수에게 사과를 구매한다."*
OOP에서는 나, 과일장수, 사과라는 객체를 등장시켜서 사과구매라는 행위를 실체화 한다.
Java에서는 객체를 `class` 라는 키워드로 표현하며 이러한 객체는 **변수(데이터)** 와 **Method(기능/행위)** 로 이루어진다.

```java
//사과 구매자가 사과장수에게서 사과를 구매했다.
class FruitBuyer{
	int myMoney = 10000; //돈(data)
	int numOfApple = 0;  //사과갯수(data)

    //사과를 산다(행위)
	public void buyApple(FruitSeller seller, int money){
		numOfApple += seller.saleApple(money);
		myMoney -= money;
	}

	//가계부를 쓴다.
	public void resultBuyer(){
		System.out.println("남은 돈: " + myMoney);
		System.out.println("내가 산 사과 갯수: " + numOfApple);
	}
}

class FruitSeller {
	int numOfApple = 100;	//사과(data)
	int myMoney = 0;  //이윤(data)
	final int APPLE_PRICE = 1000;  //변수형 상수

    //사과를 판다(행위)
	public int saleApple(int money){
		int num = money / APPLE_PRICE;
		numOfApple -= num;
		myMoney += money;
		return num;
	}

	//정산의 기능
	public void resultSeller(){
		System.out.println("이득: " + myMoney);
		System.out.println("남은 사과 갯수: " + numOfApple);
	}
}

public class FruitMain {
	public static void main(String[] args) {
		FruitBuyer buyer = new FruitBuyer();
		FruitSeller seller = new FruitSeller();

		buyer.buyApple(seller, 2000);
		buyer.resultBuyer();
		seller.resultSeller();
	}
}

```

```
남은 돈: 8000
내가 산 사과 갯수: 2
이득: 2000
남은 사과 갯수: 98
```

FruitBuyer 라는 객체를 `class` 라는 키워드로 만들어서 그 안에 데이터와 행위에 해당하는 변수(myMoney, numOfApple), 메소드(buyApple)를 지정해주었으며 이처럼 객체안의 변수와 메소드를 멤버변수, 멤버메소드라고 부른다. 이렇게 만들어진 객체를 사용하려면 메모리 공간을 할당해 주어야 되는데 *FruitBuyer buyer = new FruitBuyer()* 이렇게 선언해주며 인스턴스를 생성 한다고 한다.
이 의미는 FruitBuyer라는 타입을 가진 buyer의 이름으로 참조 변수를 선언해주고, `new` 라는 키워드로 FruitBuyer라는 객체의 메모리 공간을 할당해준다는 것이다. 이것은 `int num = 10` 과 같은 원리로 int라는 기본형과 FruitBuyer라는 사용자 정의형이라는 차이점이 있을뿐이고, 그러므로 객체도 java에서 제공하는 8가지 기본형(Primitive Type)처럼 이름이 int, char가 아닐뿐, 사용자가 정의해주는 이름을 가지는 타입이다.
그리고 이러한 사용자 정의형을 **참조 타입(Reference Type)** 이라고 한다.

```java
public void buyApple(FruitSeller seller, int money){
  numOfApple += seller.saleApple(money);
  myMoney -= money;
}
```

그러므로 위 처럼 메소드의 참조 변수로 참조 타입을 넣어주는 것도 가능하다.

[객체지향 개념 잡기](http://www.slideshare.net/plusjune/ss-46109239)

###Method

자바 프로그램의 시작은 `main` 이라는 이름의 Method를 실행하는 데서 부터 시작한다.  Method의 중괄호 내에 존재하는 문장들이 위에서 아래로 순차적으로 실행된다. `return`은 **값의 반환** 과 **Method의 종료** 라는 의미를 가진다.

![method]({{ "/assets/img/posts/java_method.PNG" }})

###변수형 상수

`final int APPLE_PRICE = 1000` 이 처럼 초기화 값만 선언하고, 값을 변경할 수 없는 변수를 **변수형 상수** 라고 하며 `final`이라는 키워드를 붙여서 사용하고 변수명은 전부 대문자를 사용한다.

###생성자(Constructor)

*생성자란 클래스의 이름과 동일한 이름의 메소드* 로 인스턴스 생성시 딱 한번 JVM에 의해 자동으로 호출된다. 인스턴스 변수의 초기화를 목적으로 정의하며 반환형이 선언되어 있지 않으면서, 반환하지 않는 메소드이다. FruitBuyer buyer = new FruitBuyer() 이 코드는 `new`로 FruitBuyer 객체를 생성하고 객체와 같은 이름인 `FruitBuyer()` 메소드를 호출하고 있다. 즉 **자바의 인스턴스 생성시에는 반드시 생성자가 호출** 되며 생성자를 정의하지 않으면 자동으로 빈 생성자 `public FruitSeller() {}` 가 삽입된다. 멤버변수의 초기화는 생성자 안에서 하는걸 권고한다.

###정보 은닉 & 캡슐화

**캡슐화(Encapsulation)** 는 관련있는 모든 메소드와 변수를 하나의 클래스로 묶는 것이다.
`private` 라는 키워드를 사용하여 **정보를 은닉(Information Hiding)** 한다. 외부 객체가 특정 객체의 데이터와 함수를 직접 접근하여 사용하거나 변경하지 못하므로 유지보수와 소프트 웨어 확장 시 오류를 최소화할 수 있다.

###접근 제어 지시자

**접근 제어 지시자(Access Control Specifiers)** 는 접근의 허용 범위를 제한하는 용도로 사용되며, `public`, `protected`, `private` 와 같은 키워드가 있다.

- private - 클래스 내부(메소드)에서만 접근 가능
- public - 어디서든 접근 가능
- protected - 상속 관계에서도 접근 가능
- default - 선언하지 않은 경우로 동일 패키지 내에서의 접근 허용

![access control specifiers]({{ "/assets/img/posts/java_access.PNG" }})

- default 클래스 - 동일한 패키지 내에 정의된 클래스에 의해서만 인스턴스 생성이 가능하다.
- public 클래스 - 하나의 소스 파일에 하나의 class만 public으로 선언이 가능하며 이때 public 클래스 이름과 소스파일 이름이 일치해야된다.
- default 생성자 - 디폴트 생성자의 접근 제어 지시자는 클래스의 선언 형태에 따라 결정된다.

###static

**static** 변수란 인스턴스의 생성과 상관없이 초기화되는 변수로, **JVM이 클래스 정보를 로딩하는 시점에서 static 변수가 초기화 되기 때문에 인스턴스 생성 없이도 사용이 가능하다.** 인스턴스의 이름과, 클래스의 이름으로 접근이 가능한데 클래스 이름으로 접근하는걸 권고한다. 동일한 클래스의 인스턴스들 사이에서 데이터 공유가 필요할 떄 static 변수를 유용하게 사용할 수 있다.
static 메소드도 변수와 동일하며 static 메소드는 인스턴스에 속하지 않기 때문에 인스턴스 멤버 혹은 인스턴스 메소드에 접근이 불가능하다.

##클래스들의 관계

###Package

```java
package com.test;

public class TestPkg {
	public TestPkg() {
		System.out.println("D");
	}

}
```

패키지는 폴더와 비슷한 개념으로 같은 이름의 파일이라도 다른 폴더내에서는 존재할 수 있듯이 변수나 클래스의 이름 중복을 피하기위해 사용된다.
패키지 명은 보통 그 회사의 도메인 명을 사용하며 `com.test` 이렇게 만든다면 " . "을 기준으로 폴더가 만들어지고 이 경우에 소스파일은 *project_name/src/com/test/* 밑에 생성이 된다.
다른 소스에서 접근 할 경우에는 `import` 키워드를 이용하여 `import com.test.TestPkg` 를 입력해준다.

###Method Overloading

메소드 오버로딩이란(Overloading) 동일한 이름의 메소드를 둘 이상 동시에 정의하는 것을 말한다. 메소드의 매개변수 선언(개수 또는 자료형)이 다르면 오버로딩이 성립되며 반환형이 다른 것은 성립이 안된다.

*주의사항*

```java
class MethodOverload {
    void  isYourFunc( int n ) { … }
    void  isYourFunc( int  n1, int  n2 ) {…}
    void  isYourFunc( int  n1, double  n2 ) {…}
}

    MethodOverload inst = new MethodOverload();
    inst.isYourFunc(10,  ‘a’);
```

위의 코드는 에러를 발생 시키지 않는다. `inst.isYourFunc(10,  ‘a’)` 이 코드는 자동형변환이 되어 문자 'a'는 int형으로도, double형으로도 변환이 가능하기 때문에 애매한 상황이 발생하게 된다.
결론적으로 형변환 규칙을 적용하되 가장 가까운 위치의 자료형으로 변환이 이루어져서 isYourFunc(int n1, int n2)가 호출된다.
그러므로 형변환의 규칙까지 적용해야만 메소드가 구분되는 애매한 상황은 만들지 말자!
생성자도 오버로딩의 대상이며 `this` 키워드를 이용하여 `this.var` 멤버 변수를 호출하거나 `this(var1, var2)` 생성자 내의 다른 생성자를 호출할 수 있다.

###String

Java는 큰 따옴표로 묶여서 표현되는 문자열을 모두 인스턴스화 하며 문자열은 String 이라는 이름의 클래스로 표현된다.
**String 인스턴스는 상수 형태의 인스턴스이기 때문에 저장된 문자열의 내용은 변경이 불가능하다.**

- `a.length()` - 문자열 길이 반환
- `a.compareTo(b)` - 문자열 비교(같으면 0 반환)
- `a.concat(b)` - a, b 문자열 연결된 문자열 반환
- `StringBuilder` - 문자열의 저장 및 변경을 위한 메모리 공간을 지니는 클래스로 문자열 데이터의 추가를 위한 append와 삽입을 위한 insert 메소드를 제공한다. 문자열의 연결이 많은 경우 `concat()` 보다 `StringBuilder` 메소드의 사용이 높은 퍼포먼스를 낸다.
- `StringTokenizer` - 구분자를 통해 문자열을 나눌 때 사용한다.

```java
import java.util.StringTokenizer;

public class StrInstance {
	public static void main(String[] args) {
		String str = "acx:ddd:xxx";
		StringTokenizer st = new StringTokenizer(str, ":");

		while(st.hasMoreTokens())
			System.out.println(st.nextToken());
	}
}
```

```
//결과
acx
ddd
xxx
```

###콘솔 입력과 출력

####출력

- `println` - 출력 후에 개행한다.
- `print` - 출력 후에 개행을 하지 않는다.
- `printf` - 서식문자로 문자열을 조합해서 출력한다.
- println, print 메소드의 인자로 인스턴스의 참조값이 전달될 수 있으며 인스턴스의 toString 메소드가 반환하는 문자열이 출력된다.

*printf example*

```java
System.out.printf("정수는 %d, 실수는 %f, 문자는 %c", 10, 2.3, 'A');
```

```
정수는 10, 실수는 2.3, 문자는 A  //결과
```

![format]({{ "/assets/img/posts/java_printf.PNG" }})

####입력

**Scanner** 클래스는 단순히 키보드의 입력만을 목적으로 디자인된 클래스가 아니라 다양한 리소스를 대상으로 입력을 받을 수 있도록 정의된 클래스이다.
`Scanner(File source)`, `Scanner(InputStream source)`, `Scanner(Readable source)`, `Scanner(String source)`

```java
Scanner scan = new Scanner(System.in);
int num = scan.nextInt();
String str = scan.next();
```

###배열(Array)

![array]({{ "/assets/img/posts/java_array.PNG" }})

*int[] arr = new int[]{1, 2, 3}* , *int[] arr = {1, 2, 3}* 이 두 개의 코드는 선언과 동시에 초기화한다.

*SuperMagic[] sm = new SuperMagic[5]* 이렇게 클래스 기반의 배열도 선언 가능하며, 이처럼 **인스턴스 배열에는 인스턴스가 생성되어 저장되는 것이 아니라 인스턴스의 참조값이 저장된다.**
그러므로 인스턴스의 생성은 따로 해줘서 인스턴스 배열에 그 인스턴스의 참조값을 할당해주어야한다.

```java
Friend[] fr = new Friend[3];

for(int i=0; i<fr.length; i++){
	fr[i] = new Friend("friend"+i);
}

//Friend 클래스의 배열 fr을 생성하고 반복문을 통해 인스턴스를 생성해주고있다.
//결과는 friend0, friend1, friend2 가 찍힐 것이다.
```

###상속(Inheritance)

상속이란 부모 클래스가 가지고있는 모든것을(생성자 제외) 자식클래스가 물려받아 같이 공유하며 나아가 확장(extends)하는 개념이다. 부모 클래스를 상위 클래스(superclass)로 부르며 상속받는 자식 클래스를 하위클래스(subclass)라고 부른다.
확장(extends)한다는 말은 부모클래스에서 가지고있는 추상적인 메소드를 자식클래스에서 구체적인 메소드로 오버라이드 할수 있기 때문에 자바에서는 상속을 정의할때 확장(extends)한다 라고 정의한다.

![inheritance]({{ "/assets/img/posts/java_inheritance.PNG" }})
![inheritance]({{ "/assets/img/posts/java_inheritance2.PNG" }})

*결론:*
- 하위 클래스의 생성자는 상위 클래스의 인스턴스 변수를 초기화 할 데이터까지 인자로 전달 받아야한다.
- 하위 클래스의 생성자는 상위 클래스의 생성자 호출을 통해서 상위 클래스의 인스턴스 변수를 초기화한다.
- 키워드 super는 상위 클래스의 생성자 호출에 사용되며, super와 함께 표시된 전달되는 인자의 수와 자료형을 참조하여 호출 할 생성자가 결정된다.

```java
class AAA{
  int num1;   // AAA(){}
}
class BBB extends AAA{
  int num2;   // BBB(){ super(); }  자동으로 삽입되는 디폴트 생성자의 형태
}
```

결과적으로 **상위 클래스의 생성자는 반드시 호출이 이루어지며 super()** 코드의 가독성을 위해서 항상 생성자를 만들어 주는걸 권고한다.
private 멤버도 상속은 되지만 접근이 불가능하며, `getter()` `setter()` 같은 메소드를 통해 접근해야한다. 이러한 메소드를 통해 값에 대한 유효성 검사를 처리하여 적합한 변수만 업데이트한다.

```java
class Man {
	private String name;

	Man(String name){
		this.name = name;
	}
	protected void changeName(boolean a, String name){
		if(!a)    //유효성 검사
			return;
		this.name = name;
	}
}
```

*상위 클래스의 static 변수를 하위 클래스도 그냥 이름만으로 접근이 가능하다.*

####상속을 위한 조건

- 상속 관계에 있는 두 클래스 사이에는 IS-A 관계가 성립해야 한다.
- IS-A 관계가 성립하지 않는 경우에는 상속의 타당성을 면밀히 검토해야 한다.
- IS-A 이외에 HAS-A 관계도 상속으로 표현 가능하다. 그러나 HAS-A를 대신해서 Composition 관계를 유지하는 것이 보다 적절한 경우가 많다.

IS-A, HAS-A : A is a B 즉 *A는 B다* 라는 관계가 성립되야 한다. HAS-A는 *A는 B를 가지고 있다.* 이 경우 A는 B를 가질 수도 있고 가지지 않을 수도 있기 때문에 이러한 경우 강한 연결고리를 갖는 상속을 하지 않고 복합관계로 표현한다.

*복합관계 example*

```java
class Gun {
	int bullet;    	// 장전된 총알의 수

	public Gun(int bnum){bullet=bnum;}
	public void shut() {
		System.out.println("BBANG!");
		bullet--;
	}
}

class Police  {
	int handcuffs;	     // 소유한 수갑의 수
	Gun gun;

	public Police(int bnum, int bcuff) {

		if(bnum != 0){
			gun = new Gun(bnum);
		} else {
			gun = null;
		}

		handcuffs=bcuff;
	}
	public void putHandcuff() {
		System.out.println("SNAP!");
		handcuffs--;
	}

	public void shut(){
		if(gun == null){
			System.out.println("KKKKK");
		} else {
			gun.shut();
		}
	}
}
```


- 모든 클래스는 Object 클래스를 상속한다.
- `final` 이 사용된 클래스는 상속되는 것을 허용하지 않는다.
- `final` 이 사용된 메소드는 상속은 허용하되 오버라이딩은 허용하지 않는다.

####상속을 설계할 때 고려할 사항

1. 모든 상속은 is-a 관계이어야 한다. 자식은 부모보다 더 구체적인 버전이어야 한다.
2. 재사용과 미래의 재사용이 가능하도록 클래스 계층구조를 설계한다.
3. 고려중인 도메인에서 클래스와 객체가 판별됨에 따라 그들의 공통점을 찾는다. 공통적인 기능들을 클래스 계층구조에서 일관성과 유지 용이성이 적절한 만큼 가능한 높이 위치시킨다.
4. 메소드를 자식의 기능에 맞도록 적절하게 오버라이딩한다.
5. 필요한 만큼 자식 클래스에 새로운 변수를 추가한다. 그러나 상속된 변수를 재정의 하지 않는다.
6. 각 클래스가 자신의 데이터를 관리하도록 한다. 따라서 필요하다면 부모의 구성자를 호출하거나 오버라이딩 된 메소드를 호출하는 데 super를 사용한다.
7. 여러 역할을 하는 클래스를 생성하는데(다중 상속이 필요할 때) 인터페이스들을 사용한다.
8. 미래에도 유용할 수 있도록 응용의 요구에 부합하는 클래스 계층구조를 설계한다.
9. 현재 사용하지 않더라도 toString 이나 equals 같은 일반적인 메소드들은 상속된 버전이 나중에 의도하지 않은 문제를 야기하지 않도록 자식 클래스에서 적절하게 재정의한다.
10. 계층구조에서 하위에 있는 구체적 클래스들을 위하여 공통적인 클래스 인터페이스를 명시하는 데 추상 클래스를 사용한다
11. 캡슐화를 위반하지 않고 파생 클래스에서 필요한 접근을 하도록 final 상수를 주의 깊게 사용한다.

###Method Overriding

**메소드 오버라이딩(Method Overriding)** 이란 *상속의 관계에서* 상위 클래스에 정의된 메소드의 이름, 반환형, 매개 변수 선언까지 완전히 동일한  메소드를 하위 클래스에서 다시 정의하는 것을 말한다.
이 때 **하위 클래스에 정의된 메소드에 의해 상위 클래스의 메소드는 가려진다.**

```java
class Speaker {
	private int volumeRate;

	Speaker(int volumeRate){
		this.volumeRate = volumeRate;
	}

	public void setVolume(int vol){
		volumeRate = vol;
	}

	public void showVolume(){
		System.out.println("볼륨 값: " + volumeRate);
	}
}

class BaseSpeaker extends Speaker{
	private int baseRate;

	BaseSpeaker(int vol, int baseRate){
		super(vol);
		this.baseRate = baseRate;
	}

	public void setBaseRate(int base){
		baseRate = base;
	}

	public void showVolume(){ 	// 오버라이딩
		System.out.println("중저음 볼륨 값: " + baseRate);
		super.showVolume();
	}
}

public class Overriding {
	public static void main(String[] args) {
		BaseSpeaker bs = new BaseSpeaker(7,10);
		bs.showVolume();

		Speaker sp = new BaseSpeaker(5, 3);   // 다형성
		sp.setBaseRate(3);		// 에러  

    // sp가 참조하는 것은 Speaker의 인스턴스로 인식하기 때문에
    // BaseSpeaker의 멤버에 접근이 불가능하다.

	}
}
```

Speaker 의 `showVolume()` 메소드를 BaseSpeaker에서 오버라이딩하고 있으며 오버라이딩된 메소드를 호출하기 위해서는 `super` 키워드를 사용한다.
**Speaker sp = new BaseSpeaker(5, 3)** 이 코드는 자바의 중요한 특성인 다형성을 의미하는데, 베이스 스피커를 스피커라고 부를 수 있지만 스피커로 베이스스피커가 갖는 기능을 조작못하는 것과 같은 의미다.
그러므로 `sp.setBaseRate(3)` 은 에러가 발생한다.

####instanceof 연산자

참조변수가 참조하고 있는 인스턴스의 실제 타입을 알아보기 위해 `instanceof` 연산자를 사용한다.
주로 조건문에 사용되며, instanceof의 왼쪽에는 참조변수를 오른쪽에는 타입(클래스명)이 피연산자로 위치한다. 그리고 연산의 결과로 boolean값인 true, false 중의 하나를 반환한다.
instanceof를 이용한 연산결과로 true값을 얻었다는 것은 참조변수가 검사한 타입으로 형변환이 가능하다는 것을 뜻한다.

###친구 관리 예제

MVC패턴으로 친구 관리 프로그램을 만들어본다.

*main (view)*

```java
package com.model;

import java.util.Scanner;

public class MyFriendBook {

	public static void main(String[] args) {
		Controller controller = new Controller(10);

		while(true){
			System.out.println("*** 메뉴 선택 ***");
			System.out.println("1. 고교 친구 정보 저장");
			System.out.println("2. 대학교 친구 정보 저장");
			System.out.println("3. 친구 전체 정보 출력");
			System.out.println("4. 친구 기본 정보 출력");
			System.out.println("5. 프로그램 종료");
			System.out.print("번호를 선택하세요 >> ");

			Scanner scan = new Scanner(System.in);
			int choice = scan.nextInt();

			switch(choice){
				case 1:
					//System.out.println("고교 친구 정보 입력");
					controller.addFriendInfo(choice);
					break;
				case 2:
					//System.out.println("대학교 친구 정보 입력");
					controller.addFriendInfo(choice);
					break;
				case 3:
					//System.out.println("친구 전체 정보 출력");
					controller.showAllData();
					break;
				case 4:
					//System.out.println("친구 전체 정보 출력");
					controller.showBasicData();
					break;
				case 5:
					System.out.println("프로그램 종료");
					return;
				default:
					System.out.println("잘못 입력하셨습니다.");
					break;
			}
		}
	}

}

```

*Friend model*

```java
package com.model;

public class Friend {
	String name;
	String phoneNum;
	String addr;

	Friend(String name, String phoneNum, String addr){
		this.name = name;
		this.phoneNum = phoneNum;
		this.addr = addr;
	}

	public void showData(){
		System.out.println("이름: "+name);
		System.out.println("핸드폰 번호: "+phoneNum);
		System.out.println("주소: "+addr);
	}
	public void showBasicData(){}		//오버라이딩
}
```

*고등학교 친구 model*

```java
package com.model;

public class HighFriend extends Friend{
	String work;

	public HighFriend(String name, String phoneNum, String addr, String work){
		super(name, phoneNum, addr);
		this.work = work;
	}

	public void showData(){
		super.showData();
		System.out.println("직업: "+work);
	}

	public void showBasicData(){
		System.out.println("이름: "+name);
		System.out.println("핸드폰 번호: "+phoneNum);
	}
}
```

*대학교 친구 model*

```java
package com.model;

public class UnivFriend extends Friend{
	String major;

	public UnivFriend(String name, String phoneNum, String addr, String major){
		super(name, phoneNum, addr);
		this.major = major;
	}

	public void showData(){
		super.showData();
		System.out.println("전공: "+major);
	}

	public void showBasicData(){
		System.out.println("이름: "+name);
		System.out.println("핸드폰 번호: "+phoneNum);
		System.out.println("전공: "+major);
	}
}
```

*Controller*

```java
package com.model;

import java.util.Scanner;

public class Controller {
	private Friend[] myFriend;
	private int cnt;

	public Controller(int num){
		myFriend = new Friend[num];
		cnt = 0;
	}

	private void addFriendSave(Friend friend){
		myFriend[cnt] = friend;
		cnt++;
	}

	public void addFriendInfo(int choice){
		String name, phoneNum, addr, work, major;

		Scanner input = new Scanner(System.in);
		System.out.print("이름: ");
		name = input.nextLine();
		System.out.print("전화번호: ");
		phoneNum = input.nextLine();
		System.out.print("주소: ");
		addr = input.nextLine();

		if(choice == 1){
			System.out.print("직업: ");
			work = input.nextLine();
			HighFriend friend = new HighFriend(name, phoneNum, addr, work);

			addFriendSave(friend);
		} else {
			System.out.print("전공: ");
			major = input.nextLine();
			UnivFriend friend = new UnivFriend(name, phoneNum, addr, major);

			addFriendSave(friend);
		}
		System.out.println("친구 정보 저장 완료\n");
	}

	public void showAllData(){
		System.out.println("--------친구 데이터---------");
		for(int i=0; i < cnt; i++){
			myFriend[i].showData();
			System.out.println("-----------------------");
		}
		System.out.println("친구 출력 완료\n");
	}

	public void showBasicData(){
		for(int i=0; i < cnt; i++){
			myFriend[i].showBasicData();
			System.out.println("-----------------------");
		}
	}
}
```

###abstract

**추상 클래스는 추상적인 내용만 정의하고 있는 클래스로 구체적인 내용은 하위 클래스에서 구현되도록 해놓은 클래스다.**
추상 클래스가 반드시 추상 메소드를 가지고 있어야 된다는 제약이 있는 것은 아니지만 일반적으로 하나 이상의 추상 메소드를 가지고 있다. 추상 클래스의 자식 클래스는 부모로부터 상속받은 모든 추상 메소드들을 정의해야 한다. 그렇지 않으면 자식 역시 추상 클래스가 된다.
하나 이상 abstract 메소드를 포함하는 클래스는 abstract로 선언되어야 하며 인스턴스 생성은 불가능하다.

###interface

상속의 개념은 클래스뿐만 아니라, 인터페이스에도 적용될 수 있다. 즉 하나의 인터페이스는 다른 인터페이스로부터 파생될 수 있다는 것이다. 이러한 관계는 클래스 계층구조와 비슷하게 인터페이스 계층구조를 형성한다.
인터페이스는 다중상속을 지원하지 않는 자바에서 다중상속과 유사한 기능을 구현할 수 있게 한다.
**인터페이스는 멤버변수와 메소드로 구성되는데 멤버변수는 모두 상수형으로 선언되고 메소드는 모두 추상 메소드로 선언된다.**
인터페이스를 구현하고자 하는 클래스는 선언할 때 `implements`를 이용해서 인터페이스를 지정하고, 지정된 인터페이스가 가지고 있는 추상 메소드를 오버라이딩해야 한다.
클래스는 인터페이스 유형으로 변환될 수 있는데, 클래스가 구현하고 있는 인터페이스 유형으로만 변환할 수 있다.
클래스가 상속 관계에 따라 계층 구조를 가지듯이, 인터페이스도 인터페이스 사이의 상속 관계에 따라 계층 구조를 가질 수 있는데, 인터페이스의 경우는 클래스와는 다르게 여러 개의 인터페이스로부터 상속 받을 수 있다.

- 인터페이스는 두 결과물의 연결고리가 되는 일종의 약속 역할을 한다.
- 인터페이스는 둘 이상을 동시에 구현 가능.
- 인터페이스의 구현은 extends가 아닌 implements를 사용한다.
- 인터페이스 간 상속이 가능하며 이때는 extends를 사용한다.


###Inner, Nested, Local, Anonymous 클래스

클래스의 정의가 다른 클래스의 내부에 삽입될 수 있다. 이 때 외부의 클래스를 가리켜 *Outer 클래스* 라고 하고, 내부의 클래스를 가리켜 *Inner 클래스* 라고 한다.
Inner 클래스의 형태에 `static` 선언이 삽입되면 이를 가리켜 *Nested 클래스* 라고 한다.
중요한건 **Anonymous 클래스** 로 이벤트 처리에 많이 사용되니 (특히 안드로이드) 기억해 두어야 한다.


- Outer 클래스의 인스턴스를 생성한 후에야 Outer 클래스의 참조변수로 Inner 클래스의 인스턴스 생성이 가능하다.
- Inner 클래스 내에서는 Outer 클래스의 멤버에 직접 접근이 가능하다.
- Local 클래스는 메소드 내에 정의가 되어서, 메소드 내에서만 인스턴스의 생성 및 참조 변수의 선언이 가능하다는 특징이 있다.

####Anonymous 익명 클래스

익명 클래스는 특이하게도 다른 내부 클래스들과 달리 이름이 없다. **클래스의 선언과 객체의 생성을 동시에 하기 때문에 단 한번만 사용될 수 있고 오직 하나의 객체만을 생성할 수 있는 일회용 클래스이다.** 이름이 없기 때문에 생성자도 가질수 없으며, 조상클래스의 이름이나 구현하고자 하는 인터페이스의 이름을 사용해서 정의하기 때문에 하나의 클래스로 상속받는 동시에 인터페이스를 구현하거나 둘 이상의 인터페이스를 구현할 수 없다. 오로지 단 하나의 클래스를 상속받거나 단 하나의 인터페이스만을 구현할 수 있다.

```java
class InnerEx6 {
	Object iv = new Object(){ void method(){} };		// 익명클래스
	static Object cv = new Object(){ void method(){} };	// 익명클래스


	void myMethod() {
		Object lv = new Object(){ void method(){} };	// 익명클래스
	}
}
```
