``` java 
# list 转 map
 Map<Integer, String> map = list.stream().collect(  Collectors.toMap(Student::getAge, Student::getName, (key1, key2) -> key1) );
```
