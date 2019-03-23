#  一 .lambda表达式

## 1.语法

一、Lambda 表达式的基础语法：

Java8中引入了一个新的操作符 "->" 该操作符称为箭头操作符或 Lambda 操作符箭头操作符将 Lambda 表达式拆分成两部分：
                                                               
左侧：Lambda 表达式的参数列表
右侧：Lambda 表达式中所需执行的功能， 即 Lambda 体

语法格式一：无参数，无返回值 
		() -> System.out.println("Hello Lambda!"); 

语法格式二：有一个参数，并且无返回值
		(x) -> System.out.println(x) 

语法格式三：若只有一个参数，小括号可以省略不写 
		x -> System.out.println(x) 

语法格式四：有两个以上的参数，有返回值，并且 Lambda 体中有多条语句
	Comparator<Integer> com = (x, y) -> {
		System.out.println("函数式接口");

​		return Integer.compare(x, y); 
​	};                                                     

语法格式五：若 Lambda 体中只有一条语句， return 和 大括号都可以省略不写
		Comparator<Integer> com = (x, y) -> Integer.compare(x, y); 

语法格式六：Lambda 表达式的参数列表的数据类型可以省略不写，因为JVM编译器通过上下文推断出，数据类型，即“类型推断” 
		(Integer x, Integer y) -> Integer.compare(x, y); 

上联：左右遇一括号省
下联：左侧推断类型省
横批：能省则省

二、Lambda 表达式需要“函数式接口”的支持 
函数式接口：接口中只有一个抽象方法的接口，称为函数式接口。 可以使用注解 @FunctionalInterface 修饰   



## 2.使用

> 所以,如果需要书写lambda表达式你需要
>
> 1. 一个函数式接口.(只有一个抽象方法):通常定义了一个泛型
> 2. 需要一个方法X的入参中有这个函数式接口.(你需要哪里调用.) 通常在这里声明了泛型
> 3. 调用方法X的时候书写lambda表达式.



而实际上,

1. java8提供了.
2. java8提供了.



1.函数式接口.

 * Java8 内置的四大核心函数式接口

    1. Consumer<T> : void accept(T t);

       消费型接口

    2. Supplier<T> : T get(); 

       供给型接口

    3. Function<T, R> : R apply(T t);

       函数型接口

    4. Predicate<T> :boolean test(T t);

       断言型接口

2. 方法X:你需要用到lamda表达式的时候大部分方法都需要lambda表达式.

   

# 方法引用

一、方法引用：若 Lambda 体中的功能，已经有方法提供了实现，可以使用方法引用

（可以将方法引用理解为 Lambda 表达式的另外一种表现形式）完全实现,没有一点差别

 * 			  （可以将方法引用理解为 Lambda 表达式的另外一种表现形式）
 * 			  1. 对象的引用 :: 实例方法名
 * 			  2. 类名 :: 静态方法名
 * 			  3. 类名 :: 实例方法名

* 4. 构造器引用  className::new
* 5. 数组引用  type[]::new

注意:  

1. 你的接口的方法的入参和返回一定要和方法引用的完全一致.

   如:我想用log.debug输出一个int,如果是在其他的地方我可以`log.debug("{}",i)`

   但是如果用lambda表达式,做不了 .你只能改变int,用int+""变成String,因为我的接口方法就只有一个入参,而log.debug没有一个入参是Int的方法.



```java
		@Test
     public void test4() {
          employees.forEach(e-> log.debug(e.getName()));
          List<Integer> collect =  employees.stream()
              .map(e -> e.getAge()).collect(Collectors.toList());
//          collect.forEach(z->log.debug("{}",z+1));
       // 关键是这句.....
          collect.forEach(test::mylog);
     }

		public static void mylog(int i) {
          log.debug(i+"");
     }
```

==我要实现的是Consumer的`void accept(T t);`我想用log.debug去输出Integer,但是没有,我就只能自己写一个.==



第三种方法,类名:: 实例方法名

```java
BiConsumer<String, String> biConsumer = (x, y) -> x.equals(y);
BiConsumer<String, String> biConsumer1 = String::equals;
```

只有这样才可以用

1. 你有俩入参.

2. 你的第一个参数是实例方法的调用者(X),你的第二个参数是实例方法的入参.

你可以使用==类名::实例方法名==



构造器引用  类名::new

```java
//入参为空,返回为Employee. 
Supplier<Employee> supplier = Employee::new;
```



数组引用 类名[]::new

```java
		Function<Integer, Employee[]> fun2 = Employee[] :: new;
```



# 二. Stream API

## 1. 获取Steam

```java
		//1. Collection 提供了两个方法  stream() 与 parallelStream()
		List<String> list = new ArrayList<>();
		Stream<String> stream = list.stream(); //获取一个顺序流
		Stream<String> parallelStream = list.parallelStream(); //获取一个并行流
		
		//2. 通过 Arrays 中的 stream() 获取一个数组流
		Integer[] nums = new Integer[10];
		Stream<Integer> stream1 = Arrays.stream(nums);
		
		//3. 通过 Stream 类中静态方法 of()
		Stream<Integer> stream2 = Stream.of(1,2,3,4,5,6);
		
		//4. 创建无限流
		//迭代 从0开始一直加2 一直下去.
		Stream<Integer> stream3 = Stream.iterate(0, (x) -> x + 2).limit(10);
		stream3.forEach(System.out::println);
		
		//生成
		Stream<Double> stream4 = Stream.generate(Math::random).limit(2);
		stream4.forEach(System.out::println);
```



## 2. 中间操作

![image-20190319111751426](/Users/zhangzhaoxiong/笔记/java-base/resources/java8-Stream-中间操作.png)



![image-20190319111844395](/Users/zhangzhaoxiong/笔记/java-base/resources/java8-Stream-中间操作2.png)

![image-20190319111916246](/Users/zhangzhaoxiong/笔记/java-base/resources/java8-Stream-中间操作3.png)

![image-20190319152703400](/Users/zhangzhaoxiong/笔记/java-base/resources/java8-stream-终止操作.png)

> 
>
> 中间操作
>
> 1. filter 用断言式接口 排除一些元素
>
> 2. limit 截断流,使其元素不会超过个数.
>
> 3. skip 跳过元素,返回一个扔掉前n个元素的流.若流中元素不足n个.则返回一个空流.  
>
> 4. distinct 筛选,通过流所生成元素的hashCode和equals方法去除重复元素(所以需要重写equals和hashcode方法)
>
> 5. map 接收lambda,将元素转换成其他形式或提取信息.接收一个函数作为参数,该函数会应用到每个元素上并将其映射生成一个新的元素.
>
> 6. flatMap 接收一个函数作为参数.将流中的的每个值都换成另外一个流,然后把所有的流连接成一个流.
>
> (如果你的方法返回的就是一个流,你可以用这个去拼起来.)像list.add和addall
>
> 7. sorted()  自然排序
>
> 8. sorted(comparator) 定制排序
>
> 终止操作
>
> 1. allMatch——检查是否匹配所有元素  返回boolean
>
> 2. anyMatch——检查是否至少匹配一个元素  返回boolean
>
> 3. noneMatch——检查是否没有匹配的元素 返回boolean
>
> 4. findFirst——返回第一个元素  返回Option
>
> 5. findAny——返回当前流中的任意元素 (串行流返回第一,并行流随机). 建议是有paralleStream并行流去找
>
> 6. count——返回流中元素的总个数
>
> 7. max——返回流中最大值
>
> 8. min——返回流中最小值
>
> 9. reduce —— 归约 (类型不变)
>
>    ```java
>    List<Integer> list = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
>    Integer reduce = list.stream().reduce(0, (x, y) -> x + y);
>    System.out.println(reduce);
>    流是什么类型的,reduce就是什么类型的.
>    ```
>
> 10. 收集. 将Stream统一汇总,转换为其他形式.
>
>     ```java
>     // 转换为其他形式的集合
>     employees.stream().collect(Collectors.toCollection(HashSet::new));
>     // 求平均值
>     employees.stream().map(Employee::getSalary).collect(Collectors.averagingDouble(Integer::doubleValue));
>     employees.stream().collect(Collectors.averagingDouble(Employee::getSalary));
>     // 转换成Map 一个方法提供Key,一个方法提供value
>     Map<String, Integer> collect = employees.stream().filter(employee -> employee.getAge() > 30)
>                 .collect(Collectors.toMap(Employee::getName, Employee::getSalary));
>     
>     collect.forEach((k,v)->{
>       log.debug("key:{}--value:{}",k,v);
>     });
>     // 可以求总和 summing
>     代码略过.
>     // 可以取最大值最小值  maxBy minBy 
>     employees.stream().filter(e -> e.getAge() > 20)
>        .collect(Collectors.maxBy((e1, e2) -> Double.compare(e1.getSalary(), e2.getSalary())));
>     
>     employees.stream().filter(employee -> employee.getAge() > 20)
>        .collect(Collectors.minBy((e, e1) -> Double.compare(e.getSalary(), e1.getSalary())));
>     
>     // 分组 多级分组 按你返回的Key去分组
>      Map<Integer, List<Employee>> collect1 = 
>        // 按年龄分组
>      employees.stream().collect(Collectors.groupingBy(Employee::getAge));
>     
>     Map<Integer, Map<Integer, List<Employee>>> collect1 = employees.stream()
>       // 按年龄分组然后按薪水分组
>                 .collect(Collectors.groupingBy(Employee::getAge, Collectors.groupingBy(Employee::getSalary)));
>     
>     // 分片  true/flase   partotopningBy
>     // 按true和false分组.
>     employees.stream().collect(Collectors.partitioningBy(c -> c.getSalary() > 5000));
>     
>     //一次性 全来 count,sum,min,average,max
>     DoubleSummaryStatistics collect3 = employees.stream()
>                 .collect(Collectors.summarizingDouble(Employee::getSalary));
>     
>     // 字符串连接
>     employees.stream().map(Employee::getName).collect(Collectors.joining());
>     // 中间加上, zzx,zzg,hzs
>     employees.stream().map(Employee::getName).collect(Collectors.joining(","));
>     // 首尾加字符  ***zzx,zzg,hzs===
>     employees.stream().map(Employee::getName).collect(Collectors.joining(",","***","==="));
>     ```
>
>     



## 3. forkjoin 框架

大数据可以用到.

可以参考java8 14课.



现在Java8简化了他.

```java
Instant now = Instant.now();
// 串行流(默认)
long reduce = LongStream.rangeClosed(0, 100000000000L).reduce(0, (x, y) -> x + y);
// 并行流
long reduce2 = LongStream.rangeClosed(0, 100000000000L).parallel().reduce(0, (x, y) -> x + y);

log.debug(reduce+"");
log.debug(Duration.between(now, Instant.now()).toMillis()+"");
```

串行流,并行流可以通过这两个方法之间变换.

`parallel()变成并行流`

`sequential()变成串行流`





# 三.Optional类



![image-20190319192251132](/Users/zhangzhaoxiong/笔记/java-base/resources/Java8-Optional简介.png)

 



原来的Pojo是某个对象的时候就是写某个对象.

现在可以`private Optional<Godness> godness = Optional.empty();`

然后可以通过Optional的API进行操作.



# 四.接口改动

现在接口也允许默认方法.

接口中也允许有静态方法.

```java
default void Java8Change(String string) {
        if (!string.isEmpty()) {
            System.out.println(string);
        }
    }
```

1. 如果一个类同时继承和实现的类中有相同方法. 类优先.

2. 如果实现的两个接口中有相同的默认方法.  必须要重写一个.

3. (没变)如果两个接口中有相同的未实现方法,你必须选择一个去实现.

   

# 五.全新的时间和日期

以前的时间日期类有很多,但是有各种各样的问题.

比如.可变(线程不安全),操作麻烦.



```java
public static void main(String[] args) throws Exception {
		
		SimpleDateFormat sdf = new SimpleDateFormat("yyyyMMdd");
		
		Callable<Date> task = new Callable<Date>() {

			@Override
			public Date call() throws Exception {
				return sdf.parse("20161121");
			}
			
		};

		ExecutorService pool = Executors.newFixedThreadPool(10);
		
		List<Future<Date>> results = new ArrayList<>();
		
		for (int i = 0; i < 10; i++) {
			results.add(pool.submit(task));
		}
		
		for (Future<Date> future : results) {
			System.out.println(future.get());
		}
}
// 线程不安全,直接报错.
```



新的日期.

LocalDate/LocalTime/LocalDateTime,他们是不可变的(线程安全的.)













![image-20190320115645388](/Users/zhangzhaoxiong/笔记/java-base/resources/java8-date-日期校正器.png)

```java
public class TestNewDate {

    private static final Logger log = LoggerFactory.getLogger(TestNewDate.class);

    @Test
    public void testNewDate() {

        // 构造
        LocalDateTime now = LocalDateTime.now();
        LocalDate now1 = LocalDate.now();
        LocalTime now2 = LocalTime.now();
        myPrint(now);
        myPrint(now1);
        myPrint(now2);

        LocalDateTime ofLocalDateTime1 = LocalDateTime.of(2019, 03, 20, 10, 12, 13, 300);
        myPrint(ofLocalDateTime1);
        myPrint("---------------------------");
        // 日期计算
        LocalDateTime now3 = now.plusDays(1);
        LocalDate now4 = now1.plusMonths(1);
        LocalTime now5 = now2.plusHours(1);

        LocalDateTime now6 = now.minusDays(1);
        LocalDate now7 = now1.minusDays(1);
        LocalTime now8 = now2.minusHours(1);

        myPrint(now3);
        myPrint(now4);
        myPrint(now5);
        myPrint("---------------------------");

        // 获取年月日
        myPrint(now.getDayOfMonth());
        myPrint(now.getDayOfWeek());
        myPrint(now.getDayOfYear());
        myPrint(now.getHour());
        myPrint(now.getMinute());
        // 获取月份对象
        myPrint(now.getMonth());
        // 获取月份数值 3
        myPrint(now.getMonthValue());

        myPrint("---------------------------");
        // Instant: 时间戳(以Unix元:1970年1月1日 00:00:00到某个时间之间的毫秒值) 默认是UTC时区
        myPrint(Instant.now());
        // 转成毫秒(数字)
        myPrint(Instant.now().toEpochMilli());
        OffsetDateTime offsetDateTime = Instant.now().atOffset(ZoneOffset.ofHours(8));
        // 改时区.
        myPrint(offsetDateTime);
        // 从元年开始加1min
        Instant instant = Instant.ofEpochSecond(60);
        myPrint(instant);

        // 时间间隔 Duration
        Instant instant1 = Instant.ofEpochSecond(0);
        Duration between = Duration.between(Instant.ofEpochSecond(0), Instant.ofEpochSecond(6000));
        Duration between1 = Duration.between(Instant.now(), instant1);
        myPrint("---------------------------");

        //比如相差了  2s 30纳秒
        // 结果2
        myPrint("getSeconds", between.getSeconds());
        // 结果 30
        myPrint("getNano", between.getNano());
        // 全部转化为纳秒
        myPrint("toNanos", between.toNanos());
        // 转化为天
        myPrint("toDays", between.toDays());
        // 全部转化为小时
        myPrint("toHours", between.toHours());
        // 毫秒
        myPrint("toMillis", between.toMillis());
        // 分钟
        myPrint("toMinutes", between.toMinutes());

        // 时间间隔
        myPrint("---------------------------");
//        LocalDate now9 = LocalDate.now();
//        LocalDate of = LocalDate.of(1993 , 9, 23);
        LocalTime now9 = LocalTime.now();
        LocalTime of = LocalTime.of(10, 23);

        Duration between2 = Duration.between(now9, of);
        myPrint("getSeconds", between2.getSeconds());
        // 结果 30
        myPrint("getNano", between2.getNano());
        // 全部转化为纳秒
        myPrint("toNanos", between2.toNanos());
        // 转化为天
        myPrint("toDays", between2.toDays());
        // 全部转化为小时
        myPrint("toHours", between2.toHours());
        // 毫秒
        myPrint("toMillis", between2.toMillis());
        // 分钟
        myPrint("toMinutes", between2.toMinutes());

        //日期间隔 period
        myPrint("---------------------------");
        LocalDate now10 = LocalDate.now();
        LocalDate of1 = LocalDate.of(1993, 9, 23);
        Period between3 = Period.between(of1, now10);
        myPrint(between3.getDays());
        myPrint(between3.getMonths());
        myPrint(between3.getYears());
        myPrint(between3.toTotalMonths());

    }

    private <T> void myPrint(T t) {
        log.debug("{}", t);
    }

    private <T> void myPrint(String str, T t) {
        log.debug("{}:{}", str, t);
    }

    /**
     * 时间校正器
     * @date 12:23 2019/3/20
     */
    @Test
    public void test3() {
        LocalDate now = LocalDate.now();
        myPrint(now);
        LocalDate with = now.with(TemporalAdjusters.next(DayOfWeek.SUNDAY));
        // 下个周日吧
        myPrint(with);

        myPrint(now.with(l -> {
            LocalDate l1 = (LocalDate) l;
            return l1.plusDays(45);
        }));
    }

    /**
     * 时间格式化
     * @date 12:23 2019/3/20
     */
    @Test
    public void test4() {
        LocalDateTime now = LocalDateTime.now();
        DateTimeFormatter dateTimeFormatter = DateTimeFormatter.ISO_DATE;
        String format = now.format(dateTimeFormatter);
        myPrint(format);
        // 好像不能丢失精度 不然回不来.
        DateTimeFormatter dateTimeFormatter2 = DateTimeFormatter.ofPattern("华夏yyyy年MM月dd日 HH:mm:ss");
        String format1 = dateTimeFormatter2.format(now);
        myPrint(format1);
        myPrint(now.format(dateTimeFormatter2));

        LocalDateTime parse = LocalDateTime.parse(dateTimeFormatter2.format(now), dateTimeFormatter2);
//        LocalDateTime parse1 = now.parse(format1, dateTimeFormatter2);
        myPrint(parse);
    }

    /**
     * 时区
     * @date 12:35 2019/3/20
     */
    @Test
    public void test5(){
        // 获取所有支持的时区.
        Set<String> availableZoneIds = ZoneId.getAvailableZoneIds();
//        availableZoneIds.forEach(this::myPrint);
        LocalDateTime now = LocalDateTime.now(ZoneId.of("Europe/Helsinki"));
        myPrint(now);
        LocalDateTime now1 = LocalDateTime.now();
        // 用系统的时间构建一个XX时区的时间. 
        ZonedDateTime zonedDateTime = now1.atZone(ZoneId.of("Europe/Helsinki"));


    }


    @Test
    public void personTest() {
        Date date = new Date(1993, 9, 23);
        Date date1 = new Date(2019, 3, 20);
        long t = date1.getTime() - date.getTime();

        myPrint("秒:", t / 1000);
        myPrint("分钟:", t / 1000 / 60);
        myPrint("小时:", t / 1000 / 60 / 60);
        myPrint("天:", t / 1000 / 60 / 60 / 24);
        myPrint("月:", t / 1000 / 60 / 60 / 24 / 30);
        myPrint("年:", t / 1000 / 60 / 60 / 24 / 30 / 12);
    }


}
```







# 六.注解优化

暂缓.

