# Call By Value, Call By Reference

함수가 호출될 때 인자로 전달된 값은 임시 메모리 공간에 **할당**하게 되는데 복사된 값은 지역변수이며, 함수가 끝나면 사라진게 된다.

## Call By Value(값 호출)

함수 호출시 전달되는 변수의 값을 복사하여 인자로 전달한다.

따라서 함수 안에서 인자의 값이 변경되어도 호출시에 전달된 변수의 값은 변경되지 않는다.

Java는 기본 자료형과 참조 자료형의 모두 Call By Value 방식을 따른다.

전달되는 값이 기본자료형인 경우에 값이 복사되고, 객체인 경우 객체의 주소가 복사되어 전달된다. 객체의 주소가 전달되는 것이기 때문에 call by reference로 해석하기도 한다. (엄밀히 말하면 call by value)

```java
public class Test {
	public static void main(String[] args) {
		int a = 1;
		int b = 2;

		System.out.println(a + " " + b);
		change(a, b);
		System.out.println(a + " " + b);
	}

	public static void change(int a, int b) {
		int tmp = a;
		a = b;
		b = tmp;
	}
}

### 결과 ###
1 2
1 2
```

## Call By Reference(참조 호출)

함수 호출 시 인자로 전달되는 변수가 가리키는 **객체의 주소값이 복사**되어 인자로 전달됩니다.

따라서 함수 안에서 인자의 값이 변경되면 호출시에 전달된 변수의 값도 변경된다.

```java
public class Test {
	public static void main(String[] args) {
		Integer a = 1;
		Integer b = 2;

		System.out.println(a + " " + b);
		change(a, b);
		System.out.println(a + " " + b);
	}

	public static void change(Integer a, Integer b) {
		Integer tmp = a;
		a = b;
		b = tmp;
	}
}

### 결과 ###
1 2
1 2
```

어? 왜 값이 바뀌지 않았지? 

reference가 전달된 것은 맞다. 하지만 `Integer`는 **불변 객체**기 때문에 객체 내 값을 변경할 수 없다. 따라서 change 메서드 내에서 임시로 새로운 객체를 가르키고 다시 돌아오면서 사라진다.

Java에서 대표적인 불변 객체는 String, Integer, Double등 Wrapper클래스와 java.time 패지키 하위 클래스들이 있다.

```java
public class Test {

	public static void main(String[] args) {
		Something a = new Something(1);
		Something b = new Something(2);

		System.out.println(a.value + " " + b.value);
		change(a, b);
		System.out.println(a.value + " " + b.value);
	}

	public static void change(Something a, Something b) {
		int tmp = a.value;
		a.value = b.value;
		b.value = tmp;
	}
}

class Something{
	int value;

	public Something(int value) {
		this.value = value;
	}
}

### 결과 ###
1 2
2 1
```

다음과 같이 변경된 것을 볼 수 있다. (a.value = b.value 가 아닌 a = b면 바뀌지 않는다.)

한가지 주의할 점은 call by reference 또한 임시 공간을 할당하기 때문에 실제 객체 주소값을 새로운 주소값으로  할당한다는 사실을 잊지 말자.

### 결론

- Java는 모두 Call by Value 방법이다.
- Primitive Type이냐 Reference Type이냐에 따라 전달되는 값이 다르다.
- 객체(주소)가 전달되는 경우 내부 값을 변경할 수 있다.
    - Wrapper, java.time 클래스는 불변 객체라 내부 값을 변경할 수 없다.
