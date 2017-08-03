---
title: Map集合的集中遍历方式
tags: map,遍历,Java
grammar_cjkRuby: true
---

### 遍历结合推荐使用Iterator

> 使用 entrySet 遍历 Map 类集合 KV，而不是 keySet 方式进行遍历。

>说明：keySet 其实是遍历了 2 次，一次是转为 Iterator 对象，另一次是从 hashMap 中取出 key所对应的 value。而 entrySet 只是遍历了一次就把 key 和 value 都放到了 entry 中，效率更高。如果是 JDK8，使用 Map.foreach 方法。

``` stylus

package top.xiesen.npedemo.test;

import java.util.HashMap;
import java.util.Iterator;
import java.util.Map;

public class TestIteratorMap {

	public static void main(String[] args) {
		Map<String, String> map = new HashMap<String, String>();
		map.put("1", "value1");
		map.put("2", "value2");
		map.put("3", "value3");

		// 第一种：普遍使用，二次取值
		System.out.println("通过Map.keySet遍历key和value：");
		for (String key : map.keySet()) {
			System.out.println("key= " + key + " and value= " + map.get(key));
		}

		// 第二种
		// 推荐使用entrySet遍历
		System.out.println("通过Map.entrySet使用iterator遍历key和value：");
		Iterator<Map.Entry<String, String>> it = map.entrySet().iterator();
		while (it.hasNext()) {
			Map.Entry<String, String> entry = it.next();
			System.out.println("key= " + entry.getKey() + " and value= " + entry.getValue());
		}

		// 第三种：推荐，尤其是容量大时
		System.out.println("通过Map.entrySet遍历key和value");
		for (Map.Entry<String, String> entry : map.entrySet()) {
			System.out.println("key= " + entry.getKey() + " and value= " + entry.getValue());
		}

		// 第四种
		System.out.println("通过Map.values()遍历所有的value，但不能遍历key");
		for (String v : map.values()) {
			System.out.println("value= " + v);
		}

	}

}

```
