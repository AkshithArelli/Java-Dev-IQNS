

✅ 1. Filter even numbers from a list

Input
```java
List<Integer> nums = List.of(1, 2, 3, 4, 5, 6);
```
Solution
```java
List<Integer> evens = nums.stream()
        .filter(n -> n % 2 == 0)
        .toList();
```
Output
```
[2, 4, 6]
```

⸻

✅ 2. Convert list of strings to uppercase

Input
```java
List<String> names = List.of("john", "alex", "mike");
```
Solution
```java
List<String> upper = names.stream()
        .map(String::toUpperCase)
        .toList();
```
Output
```
[JOHN, ALEX, MIKE]
```

⸻

✅ 3. Find first / any element

Input
```java
List<String> names = List.of("A", "B", "C");
```
Solution
```java
String first = names.stream().findFirst().orElse(null);
String any = names.stream().findAny().orElse(null);
```
Output
```
first = "A"
any = "A" (or any value in parallel)
```

⸻

✅ 4. Count occurrences of each word

Input
```java
List<String> words = List.of("apple", "banana", "apple", "mango");
```
Solution
```java
Map<String, Long> freq = words.stream()
        .collect(Collectors.groupingBy(w -> w, Collectors.counting()));
```
Output

{apple=2, banana=1, mango=1}


⸻

✅ 5. Find duplicates

Input
```java
List<Integer> nums = List.of(1, 2, 2, 3, 4, 4, 5);
```
Solution
```java
List<Integer> duplicates = nums.stream()
        .collect(Collectors.groupingBy(n -> n, Collectors.counting()))
        .entrySet().stream()
        .filter(e -> e.getValue() > 1)
        .map(Map.Entry::getKey)
        .toList();
```
Output
```
[2, 4]
```

⸻

✅ 6. Remove duplicates

Input
```java
List<Integer> nums = List.of(1, 2, 2, 3, 4, 4);
```java
Solution
```
List<Integer> uniq = nums.stream().distinct().toList();
```
Output
```
[1, 2, 3, 4]
```

⸻

✅ 7. Sort list of strings / objects

Input
```java
List<String> names = List.of("Charlie", "Bob", "Alex");
```
Solution
```java
List<String> sorted = names.stream()
        .sorted()
        .toList();
```
Output
```
[Alex, Bob, Charlie]
```

⸻

⚡ Sorting objects
```java
List<Employee> sorted = employees.stream()
        .sorted(Comparator.comparing(Employee::getSalary))
        .toList();
```

⸻

✅ 8. Convert List → Map

Input
```java
List<Person> people = List.of(
    new Person(1, "John"),
    new Person(2, "Alex")
);
```
Solution
```java
Map<Integer, Person> map = people.stream()
        .collect(Collectors.toMap(Person::getId, p -> p));
```
Output
```
{1=John, 2=Alex}
```

⸻

✅ 9. Find max/min salary

Input
```java
List<Employee> employees = List.of(
    new Employee("A", 5000),
    new Employee("B", 7000),
    new Employee("C", 6000)
);
```
Solution
```java
Employee max = employees.stream()
        .max(Comparator.comparing(Employee::getSalary))
        .orElse(null);
```
Output
```
Max salary employee → B
```

⸻

✅ 10. Flatten list of lists

Input
```java
List<List<Integer>> lists = List.of(
    List.of(1, 2), 
    List.of(3, 4)
);
```
Solution
```java
List<Integer> flat = lists.stream()
        .flatMap(List::stream)
        .toList();
```
Output
```
[1, 2, 3, 4]
```

⸻

✅ 11. Partition employees by age

Input
```java
List<Employee> employees = List.of(
    new Employee("A", 25), 
    new Employee("B", 35)
);
```
Solution
```java
Map<Boolean, List<Employee>> part = employees.stream()
        .collect(Collectors.partitioningBy(e -> e.getAge() > 30));
```
Output
```
{false=[A], true=[B]}
```

⸻

✅ 12. Group employees by department

Input
```java
List<Employee> employees = List.of(
    new Employee("A", "IT"),
    new Employee("B", "HR"),
    new Employee("C", "IT")
);
```
Solution
```java
Map<String, List<Employee>> map = employees.stream()
        .collect(Collectors.groupingBy(Employee::getDepartment));
```
Output
```
{IT=[A, C], HR=[B]}
```

⸻

✅ 13. Sum & average

Input
```java
List<Integer> nums = List.of(10, 20, 30);
```
Solution
```java
int sum = nums.stream().mapToInt(n -> n).sum();
double avg = nums.stream().mapToInt(n -> n).average().orElse(0);
```
Output
```
sum = 60  
avg = 20.0
```

⸻

✅ 14. reduce(): sum, max, concatenate

Input
```java
List<Integer> nums = List.of(5, 1, 3);
```
Solution
```java
int sum = nums.stream().reduce(0, Integer::sum);
int max = nums.stream().reduce(Integer.MIN_VALUE, Integer::max);
```

⸻

Concatenate

Input
```java
List<String> words = List.of("Java", "Streams");
```
Solution
```java
String combined = words.stream()
        .reduce("", (a, b) -> a + b);
```
Output
```
JavaStreams
```

⸻

✅ 15. Parallel stream

Input
```java
List<Integer> nums = List.of(1, 2, 3, 4);
```
Solution
```java
int sum = nums.parallelStream()
        .mapToInt(n -> n)
        .sum();
```
Output
```
10
```

⸻

✅ 16. Map → List / List → Map

Input
```java
Map<String, Integer> map = Map.of("A", 1, "B", 2);
```
Map → List
```java
List<String> keys = map.keySet().stream().toList();
```
Output
```
[A, B]
```

⸻

List → Map

Input
```java
List<String> names = List.of("John", "Alex");
```
Solution
```java
Map<String, Integer> result = names.stream()
        .collect(Collectors.toMap(n -> n, n -> n.length()));
```
Output
```
{John=4, Alex=4}
```

⸻

✅ 17. Find longest word

Input
```java
List<String> words = List.of("apple", "banana", "kiwi");
```
Solution
```java
String longest = words.stream()
        .max(Comparator.comparing(String::length))
        .orElse("");
```
Output
```
banana
```

⸻

✅ 18. Find second highest

Input
```java
List<Integer> nums = List.of(10, 20, 50, 40, 30);
```
Solution
```java
int second = nums.stream()
        .sorted(Comparator.reverseOrder())
        .skip(1)
        .findFirst()
        .orElse(-1);
```
Output
```
40
```

⸻

✅ 19. Unique characters from list

Input
```java
List<String> words = List.of("abc", "bcd");
```
Solution
```java
List<Character> chars = words.stream()
        .flatMap(w -> w.chars().mapToObj(c -> (char)c))
        .distinct()
        .toList();
```
Output
```
[a, b, c, d]
```

⸻

✅ 20. map() vs flatMap() Example

Input
```java
List<List<Integer>> nums = List.of(
    List.of(1, 2),
    List.of(3, 4)
);
```
map()
```java
Stream<Stream<Integer>> mapped = nums.stream().map(List::stream);
```
flatMap()
```java
List<Integer> flat = nums.stream().flatMap(List::stream).toList();
```

⸻

✅ 21. findFirst() vs findAny()
```java
String first = names.stream().findFirst().orElse(null);
String any = names.parallelStream().findAny().orElse(null);
```

⸻

✅ 22. Short-circuiting operations

Input
```java
List<Integer> nums = List.of(5, 10, 20, 100);
```
Example
```java
boolean exists = nums.stream().anyMatch(n -> n == 20);
```

⸻

23. Reverse each word in a sentence

Input
```
"I love Java"
```
Output
```
"I evol avaJ"
```
Example
```java
Arrays.stream(sentence.split(" "))
                .map(word -> (new StringBuilder(word).reverse().toString()))
                .collect(Collectors.joining(" "));
```

Note: split a sentence which have multiple spaces or tab space or new line
        ```java
        str.split("\\s+")
        ```
        
24. reverse the entire sentence (word order), NOT reverse individual words.
Input
```
"I love Java"
```
Output
```
"I evol avaJ"
```
Example
```java
Arrays.stream(sentence.split(" "))
                .reduce("", (a, b) -> b + " " + a);
```

