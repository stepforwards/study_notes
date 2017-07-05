[TOC]

## 作业

### 格式化输入书的章节

``` stylus
for (int i = 1; i < 6; i++) {
			if (i == 5) {
				break;
			}
			for (int j = 1; j < 5 + i; j++) {
				if (j == 3) {
					continue;
				}
				System.out.println("第" + i + "章,第" + j + "节");

			}
		}
```

### 打印乘法表

``` stylus
for (int i = 1; i < 10; i++) {
			for (int j = 1; j < i+1; j++) {
				System.out.print(j + "*" + i + "=" + i*j + "\t");
			}
			System.out.println();
		}
```


### 打印水仙花数

``` stylus
		int bai = 0;
		int shi = 0;
		int ge = 0;
		for (int i = 100; i < 1000; i++) {
			bai = i / 100;
			shi = i / 10 %10;
			ge = i % 10;
			
			if(bai*bai*bai + shi*shi*shi + ge*ge*ge == i){
				System.out.println(i + " 是水仙花数");
			}
			
		}
```


### 打印大小写字母

``` stylus
private static void method2() {
		char litterLetter = 'a';
		char bigLetter = 'A';
		for (int i = 0; i < 26; i++) {
			System.out.println(litterLetter + "---" + bigLetter);
			litterLetter++;
			bigLetter++;
		}
	}
	
	private static void method1() {
		int shortNum = 65;
		int bigNum = 97;
		for (int i = 0; i < 26; i++) {
			char shortLetter = (char) shortNum;
			char bigLetter = (char) bigNum;
			System.out.println(shortLetter + "------" + bigLetter);
			shortNum++;
			bigNum++;
			
		}
	}
```


### 求1-100的和

``` stylus
		int sum = 0;
		for (int i = 0; i <= 100; i++) {
			sum +=i;
		}
		System.out.println(sum);
```

### 猜数字游戏

``` stylus
		Random random = new Random();
		int number = random	.nextInt(100) + 1;
		
		System.out.println("请输入1-100之间的数");
		while(true){
			Scanner scanner = new Scanner(System.in);
			int nextInt = scanner.nextInt();
			if(nextInt==number){
				System.out.println("你真棒");
				break;
			}
			System.out.println(nextInt > number ?  "猜大了":"猜小了");
		}
```

## 数组

###  数组的三种定义方式

``` stylus
	- String [] str = new String[4];
	- String [] str1 = new String[]{"你","我","她"};
	- int [] str2 = {1,2,3,4,5,6}
```
### 数组的默认方式



### 取数组中的数据 
	- str[1];  // 取str数组中的1号索引处的值
### 遍历数组
``` stylus
	// for循环遍历数组
	for (int i = 0; i < intArr.length; i++) {
			System.out.println(intArr[i]);
		}
	//  for each,无法控制循环次数,并且没有索引
	for (int i : intArr) {
		System.out.println(i);
	}
```




