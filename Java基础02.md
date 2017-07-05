# Java基础

## if-else,if-else if-else
- if控制语句
- 当括号的内容是true的时候,执行{}中的内容

![enter description here][1]

- if--else--控制语句

![enter description here][2]

- if--else if--else 控制语句
![enter description here][3]

- 多条件判断

![enter description here][4]
## switch-case

## Scanner
- 属于引用类型,创建的时候需要固定格式**数据类型 变量 = new 数据类型();**
- 需要导入所在的包,导包格式: import java.util.Scanner;
- 创建对象 Scanner  sc = new Scanner(System.in);
- 获取输入的整数 int a = sc.nextInt();
- 获取输入的字符串 String str = sc.nextLine();
- 注意next不读空格,nextLine读取空格
## Random
- 需要导入所在的包,导包个税: import java.util.Random;
- 创建对象 Random random = new Random();
- 获取随机整数范围是[0,50); int a = random.nextInt(50);
- 获取随机小数范围是[0,1): double b = random.nextDouble();
- 如何获取一个[a,b]的随机数 int a = randrom.nextInt(b - a + 1) + a;
## while循环与do-while循环

## break
跳出当前循环

  [1]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1499165918356.jpg
  [2]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1499165988986.jpg
  [3]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1499166042540.jpg
  [4]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1499166121230.jpg