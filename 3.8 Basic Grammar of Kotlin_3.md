# 3.8 Basic Grammar of Kotlin

## 클래스와 설계



## 8.6 설계 도구

객체기향 프로그래밍은 구현(실제 Logic을 갖는 코딩)과 설계(껍데기만 있는 코딩)로 구분할 수 있다.<br>지금까지 모두 구현 중심의 기법들을 살펴보았다.<br>지금부터는 프로그래밍 설계에 사용하는 설계도구를 배워보자.

설계:<br>파일을 분류하고, 이름을 짓고, 특정 directory에 모아좋은 것 



### Package

패키지(Package): <br>클래스와 소스 파일을 관리하기 위한 directory 구조의 저장공간.<br>아래와 같이 현재 클래스가 어떤 패키지(디렉토리)에 있는지 표시한다.<br> 디렉토리가 계층 구조로 만들어져 있으면 온점(.)으로 구분해서 각 디렉토리를 모두 나열해준다.

```kotlin
package 메인 디렉터리.서브 디렉터리
class 클래스{    
}
```

*하나의 패키지에 여러 개의 파일을 생성할 수 있기 때문에 서로 관계가 있는 파일을 동일한 패키지에 만들어두면 관리가 용이하다.*



### 추상화

프로그래밍을 하기 전 개념 설계 단계에선 클래스의 이름과 클래스 안에 있을만한 기능들을 유추해서 메서드의 이름으로 먼저 나열한다. 이때 명확한 코드는 설계 단계에서 메서드 블록 안에 직접 코드를 작성하는데, 그렇지 않은 경우에 구현 단계에서 코드를 작성하도록 메서드의 이름만 작성한다. <br>이것을 **추상화(abstaract)라고 하며 abstract 키워드를 사용**해서 명시한다.

<u>구현 단계에서는 이 추상화된 클래스를 상속받아 아직 구현되지 않은 부분을 구현</u>한다.<br>다음과 같이 추상화된 Animal 클래스를 만들고 동물이 사용할 것같은 기능 중에 walk와 move를 설계한다고 가정해보자.

```kotlin
abstract class Animal{
    fun walk(){
        Log.d("abstract", "걷습니다.")
    }
    abstract fun move()
}
```

walk는 명확히 걸어가는 행위지만 새는 날아가고 고래는 수영을 하는 것과 같이 move는 동물에 따라 달라질 수 있다.<br>이렇게 앞으로 **상속받을 자식 클래스의 특징에 따라 코드가 결정될 가능성이 있다면 해당 기능들도 모두 abstract키워드로 추상화**한다. 또한, 실제 구현 클래스는 이 추상 클래스를 상속받아 아직 구현되지 않는 추상화 되어있는 기능을 모두 구현해준다. <br>추상 클래스는 독립적으로 인스턴스화 할 수 없기 때문에 구현 단계가 고려되지 않는다면 잘못된 설계가 될 수 있다.

```kotlin
class Bird : Animal(){
    override fun move(){
        Log.d("abstract", "날아서 이동한다.")
    }
    
}
```

Android의 Activity 클래스도 많은 수의 클래스를 상속받아 만들어진다.<br>이 클래스가 상속받는 클래스 중 최상위에 Context라는 클래스가 있는데, 최상위 클래스인 Context가 abstract로 설계되어 있다.



### Interface

**인터페이스(interface)**:<br>실행 코드 없이 메서드 이름만 가진 추상 클래스.<br>누군가 설계한 개념 클래스 중 **실행코드가 한 줄이라도 있으면 추상화**, **코드 없이 메서드 이름만 나열되어 있으면 인터페이스**라고 할 수 있다.<br>상속 관계의 설계 보단 외부 모듈에서 내가 만든 모듈을 사용할 수 있도록 메서드의 이름을 나열해둔 일종의 명세서로 제공된다.

인터페이스는 **interface**예약어를 사용해서 정의할 수 있고 **인터페이스에 정의된 메서드를 오버라이드해서 구현**할 수 있다.<br>Kotlin은 property도 interface 내부에 정의할 수 있는데, 대부분의 객체지향 언어에서는 지원하지 않는다.<br><u>인터페이스는 추상클래스와는 다른게 class키워드는 사용하지 않는다.</u>

```kotlin
interface 인터페이스명{
    var 변수: String
    fun 함수1()
    fun 함수2()
}
```



### Interface 만들기

```kotlin
interface InterfaceKotlin{
(생략)var variable: String //abstract키워드가 생략되어 있다.
    fun get()
    fun set()
}
```



### 클래스에서 구현하기

인터페이스를 클래스에서 구현할 때에는 생성자를 호출하지 않고 인터페이스 이름만 지정한다.

```kotlin
class KotlinImpl : InterfaceKotlin {
    override var variable: String = "init value"
    override fun get(){
        //코드 구현
    }
    override fun set(){
        //코드 구현
    }
}
```

인터페이스를 클래스의 상속 형태가 아닌 **소스코드에서 직접 구현**할 때도 있는데, **object 키워드**를 사용해서 구현해야 한다.<br>*이는 실제 안드로이드 프로젝트를 시작하면 자주 사용하는 형태이다.*

```kotlin
var kotlinImpl = object : InterfaceKotlin{
    override var variable: String = "init"
    override fun get(){
        //코드
    }
    override fun set(){
        //코드
    }
}
```

 

### 접근 제한자

Kotlin에서  정의되는 클래스, 인터페이스, 함수, 변수는 모두 접근 제한자(Visibility Modifiers)를 가질 수 있다.<br>internal 접근 제한자로 모듈 간 접근을 제한할 수 있다.

*모듈? kotlin에서 모듈은 한 번에 같이 컴파일되는 모든 파일*



**접근 제한자 종류**<br>접근 제한자는 서로 다른 파일에게 자신에 대한 접근 권한을 제공하는 것인데<br>각 변수나 클래스 이름 앞에 아무런 예약어를 붙이지 않았을 때는 기본 적으로 public 접근 제한자가 적용된다.

- **private**<br>다른 파일에서 접근할 수 없다.
- **internal**<br>같은 모듈에 있는 파일만 접근할 수 있다.
- **protected**<br>private와 같으나 상속 관계에서 자식 클래스가 접근할 수 있다.
- **public**<br>제한 없이 모든 파일에서 접근할 수 있다.



**접근 제한자 적용**<br>접근 제한자를 붙이면 해당 클래스, 멤버 프로퍼티 또는 메서드에 대한 사용이 제한된다.

**다양한 접근 제한자를 갖는 부모 클래스**

```kotlin
open class Parent{
    private val privateVal = 1
    protected open val protectedVal = 2
    internal val internalVal = 3
    val defaultVal = 4
}
```

자식 클래스 에서 부모 클래스를 상속받고 테스트한다.

```kotlin
class Child : Parent(){
    fun callVariables(){
        //privateVal은 호출 불가
        Log.d("Modifier","protected 변수의 값은 ${protectedVal}")
        Log.d("Modifier","internal 변수의 값은 ${internalVal}")
        Log.d("Modifier","기본 제한자 변수 defaultVal의 값은 ${defaultVal}")
    }
}
```



다음은 상속관계가 아닌 외부 클래스에서 Parent 클래스를 생성하고 사용해보자. 상속 관계가 아니기 때문에 public과 internal만 접근할 수 있다.

```kotlin
class Stranger{
    fun callVariables(){
        val parent = Parent()
        Log.d("Modifier","internal 변수의 값은 ${parent.internalVal}입니다.")
        Log.d("Modifier","public 변수의 값은 ${parent.defaultVal}입니다.")
    }
}
```



### Generic

제네릭(Generics)은 **입력되는 값의 타입을 자유롭게 사용**하기 위한 설계 도구이다.<br>다음은 자주 사용되는 MutableList 클래스의 원본 코드를 이해하기 쉽게 변형한 코드이다.

```kotlin
public interface MutableList<E>{
    var list: Array<E>
    ...
}
```

클래스명 앞에 <E>라고 되어 있는 부분에 String과 같은 특정 타입이 지정되면 클래스 내부에 선언된 모든 E에 String이 타입으로 지정된다.<br>우리는 컬렉션 혹은 배열에서 입력되는 값의 타입을 특정하기 위해 다음과 같이 사용한다.

```kotlin
var list: MutableList<제네릭> = mutableList0f("월", "화", "수")
```



```kotlin
fun testGenerics(){
    //String을 제네릭으로 사용했기 때문에 list 변수에는 문자열만 담을 수 있다.
    var list: MutableList<String> = mutableList0f()
    list.add("월")
    list.add("화")
    list.add("수")
    //list.add(35) //<-입력 오류 발생
    //String타입의 item 변수로 꺼내 사용할 수 있다.
    for (item in list){
        Log.d("Generic", "list에 입력된 값은 ${item}입니다.")
    }
}
```

