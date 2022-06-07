## Stream, Optional - Java 8

### Stream API - 개요

- Java 8에서 소개된 API, 객체의 집합(Collection)을 처리(process)하기 위해 사용
- Collection 원본 객체에 변화를 주지 않는다. 
- Stream API의 메서드 분류
  - Intermediate Operation (중간 연산)
  - Terminal Operation (최종 연산)

---

### Stream API - Intermediate Operation

- 개요

  - Stream을 처리하는 과정 중간에 위치하는 동작을 일컫는다. 
  - 동작의 결과로, 처리된 Stream을 반환한다. 

- 메서드

  1. map

     - Stream의 원소 각각에, 함수를 적용한다. (매핑한다. )

       ```java
       ArrayList<Integer> n = Arrays.asList(1, 2, 3, 4, 5);
       
       // n의 원소 각각에 제곱 함수를 적용, 적용된 Stream을 반환
       n.stream().map(item -> item * item);
       ```

  2. filter

     - Stream의 원소 중에, select할 원소를 지정한다. 

       ```java
       ArrayList<Integer> n = Arrays.asList(1, 2, 3, 4, 5);
       
       // n의 원소 중 짝수를 선택한다. 
       n.stream().filter(item -> item % 2 == 0);
       ```

  3. sorted

     - Stream의 원소를 정렬한다. 

---

### Stream API - Terminal Operation

- 개요

  - Stream을 처리하는 과정 끝에 위치하는 동작을 일컫는다. 
  - 연산의 결과를 반환한다. 

- 메서드

  1. collect

     - Stream을 하나로 모아서 결과를 반환한다. 

     - 무엇을, 어떻게 collect할지는 정의해야 한다. 

       - java.util.stream.Collectors 클래스에서, 사전에 정의된 방법을 사용할 수 있다. 

       ```java
       // Accumulate names into a List
       List<String> list = people.stream().map(Person::getName).collect(Collectors.toList());
       
       // Convert elements to strings and concatenate them, separated by commas
       String joined = things.stream()
           .map(Object::toString)
           .collect(Collectors.joining(", "));
       
       // Compute sum of salaries of employee
       int total = employees.stream()
           .collect(Collectors.summingInt(Employee::getSalary)));
       
       // Group employees by department
       Map<Department, List<Employee>> byDept
           = employees.stream()
           .collect(Collectors.groupingBy(Employee::getDepartment));
       ```

  2. forEach

     - Stream의 전체 원소에 대해 iterate

       ```java
       ArrayList<Integer> n = Arrays.asList(1, 2, 3, 4, 5);
       
       n.stream().forEach(elem -> System.out.println(elem * elem));
       ```
  
  3. reduce
  
     - Stream의 전체 원소를, 하나의 객체로 reduce
  
       ```java
       ArrayList<Integer> n = Arrays.asList(1, 2, 3, 4, 5);
       
       int total = n.stream().reduce(0, (sum, i) -> sum + i);
       
       // 동일한 연산
       int sum = 0;
       for (int num : n)
           sum = sum + num;
       ```

---

### Optional Class - 개요

- NullPointerException을 전부 검출하는 로직의 단점을 보완하기 위해 Java 8에서 소개된 클래스
- 단점
  - 다수의 null check를 요구하며, 이 때 로직이 복잡하다. 
  - 더러운 코드를 양산한다. 
- 패키지
  - java.util
- private 생성자 - 외부 클래스에서 객체를 생성할 수 없다. 

---

### Optional Class - Static 메서드

- empty(): Optional
  - return - 빈 Optional 객체
- of(T value): Optional\<T>
  - return - value를 갖는 Optional 객체
- ofNullable(T value): Optional\<T>
  - return - value가 null일 경우 빈 Optional 객체, 아닌 경우 value를 갖는 Optional 객체

---

### Optional Class - Instance 메서드

- **get()**
  - value가 존재한다면 value를 반환한다. 
  - 아니라면, NoSuchElementException을 던진다. 
- **ifPresent()**
  - value가 존재한다면 true를, 아니라면 false를 반환한다. 
- **orElse(T other)**
  - value가 존재한다면 value를, 아니라면 other를 반환한다. 
- **orElseThrow(Supplier<? extends X> exceptionSupplier)**
  - value가 존재한다면 value를 반환, 아니라면 exception을 던진다. 
- **filter(Predicate<? super T> predicate)**
  - value가 존재하며, predicate의 결과 값이 참일 경우 Optional을 반환한다. 
  - predicate의 결과 값이 거짓이거나, value가 존재하지 않을 경우 빈 Optional을 반환한다. 
- **map(Function<? super T, ? extends U> mapper)**

---

### Predicate, Function, Supplier

- 개요

  - Functional Interface(함수형 인터페이스)
  - 입출력의 형태가 사전에 정의되어 있으며, 로직이 구현되어 있지 않다. 

- Predicate\<T>

  - T 객체를 입력받아서, boolean 값을 출력
  - 참 거짓을 판별

- Function<T, R>

  - T 객체를 입력받아서, R 객체를 출력

- Supplier\<T>

  - T 객체를 출력

- in Java

  ```java
  import java.util.Optional;
  import java.util.function.Function;
  import java.util.function.Predicate;
  import java.util.function.Supplier;
  
  public class Test {
  
  	public void testFilter() {
  		String name = "홍길동";
  
  		Predicate<String> predicate = new Predicate<String>() {
  			@Override
  			public boolean test(String t) {
  				if (t.length() >= 2 && t.length() < 10)
  					return true;
  				return false;
  			}
  		};
  
  		Optional<String> optName = Optional.ofNullable(name).filter(predicate);
  
  		System.out.println(optName);
  	}
  
  	public void testMap() {
  		String name = "임꺽정";
  
  		Function<String, Integer> mapper = new Function<String, Integer>() {
  			@Override
  			public Integer apply(String t) {
  				return t.length();
  			}
  		};
  
  		Optional<Integer> optLen = Optional.ofNullable(name).map(mapper);
  
  		System.out.println(optLen);
  	}
  
  	public void testOrElseThrow() {
  		String name = "김아무개";
  
  		Supplier<IllegalArgumentException> exceptionSupplier = new Supplier<IllegalArgumentException>() {
  			@Override
  			public IllegalArgumentException get() {
  				throw new IllegalArgumentException("Optional is empty");
  			}
  		};
  
  		try {
  			String optName = Optional.ofNullable(name).orElseThrow(exceptionSupplier);
  			System.out.println(optName);
  		} catch (IllegalArgumentException e) {
  			e.printStackTrace();
  		}
  	}
  
  	public static void main(String[] args) {
  		Test t = new Test();
  
  		System.out.println("------------filter test-------------");
  		t.testFilter();
  		
  		System.out.println("------------map test-------------");
  		t.testMap();
  		
  		System.out.println("------------orElseThrow test-------------");
  		t.testOrElseThrow();
  	}
  
  }
  ```


---

### Reference

- [Streams API](https://www.geeksforgeeks.org/stream-in-java/)
- [Collectors (Java Platform SE 8 ) - Oracle Help Center](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html)
- [Optional class](https://www.geeksforgeeks.org/java-8-optional-class/)
- [Predicate (Java Platform SE 8 ) - Oracle Help Center](https://docs.oracle.com/javase/8/docs/api/java/util/function/Predicate.html)
- [술어 논리(Predicate Logic) - velog](https://velog.io/@mdev97/술어-논리Predicate-Logic)