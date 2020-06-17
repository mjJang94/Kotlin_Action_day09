# 객체 선언: 싱글턴을 쉽게 만들기

코틀린은 객체 선언 기능을 통해 싱글턴을 언어에서 기본적으로 지원한다. 객체 선언은 클래스 선언과 그 클래스에 속한 단일 인스턴스의 선언을 합친 선언이다.

<pre>
<code>
object payroll{
  val allEmployees = arrayListOf<Person>()
  
  fun calculateSalary(){
  
    for (person in allEmployees){
    ...
    }
   }
 }
</code>
</pre>

객체 선언은 object로 시작하고, 객체 선언은 클래스를 정의하고 그 클래스의 인스턴스를 만들어서 변수에 저장하는 모든 작업을 단 한 문장으로 처리한다.   
하지만 생성자는 객체 선언에 쓸 수 없다. 일반 클래스 인스턴스와 달리 싱글턴 객체는 객체 선언문이 있는 위치에서 생성자 호출 없이 즉시 만들어진다.
그러므로 객체 선언에는 생성자 정의가 필요 없다.   

<pre>
<code>
Payroll.allEmployees.add(Person(...))
Payroll.calculateSalary ()
</code>
</pre>

객체 선언도 클래스나 인터페이스를 상속할 수 있다. 프레임워크를 사용하기 위해 특정 인터페이스를 구현해야 하는데, 그 구현 내부에 다른 상태가 필요하지 않은 경우에 이런 기능이 유용하다.   
예로 Comparator 인터페이스를 사용한 코드를 만들어보자, Comparator은 두 인자를 받아 그중 더 큰것을 반환한다.

<pre>
<code>
object CaseInsensitiveFileComparator: Comparator<File> {
  override fun compare(file1: File, file2: File): Int {
    return file1.path.compareTo(file2.path,ignoreCase = true)
       }
    }
 
>>> val files = listOf(File("/z"), File("/a"))
>>> println(files.sortedWith(CaseInsensitiveFileComparator))

[/a, /z]
</code>
</pre>

클래스 안에서 객체를 선언할 수도 있다 예를 들어 어떤 클래스의 인스턴스를 비교한다면 Comparator를 클래스 내부에 정의하는게 더 올바르다.

<pre>
<code>
data class Person(val name: String){
  object NameComparator : Comparator<Person> {
    override fun compare (p1: Person, p2: Person): Int = p1.name.compareTo(p2.name)
   }
}

>>> val persons = listOf(Person("Bob"), Person("Alice"))
>>> println(persons.sortedWith(Person.NameComparator))
[Person(name=Alice), Person(name=Bob)]
</code>
</pre>

**이 예제에서 INSTANCE 필드의 타입은 CaseInsensitiveFileCompartor이다.**
