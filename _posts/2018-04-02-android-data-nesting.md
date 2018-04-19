---
title: Android 两层数据嵌套
date: 2018-04-02 19:02:52
tags: 
  - Android
---

最近，我正在开发一个项目，遇到这样的一个场景「有两层数据，而且他们是以嵌套的方式存在的。」就如下列的图所示。

![](https://i.loli.net/2018/04/02/5ac21931668dc.jpg)

该布局有两个列表，最外层的日期列表和内层的具体事件列表。

## 1. 使用两层 RecycleView 嵌套

当时，我第一次想到的方案是使用 `RecycleView` 作为嵌套，实现这种功能。因为这样实现起来非常方便。外层的 RecycleView 显示年份，内层的 RecycleView 显示年份内的具体事件。

撸起袖子干，当我将代码写好，准备舒服地欣赏结果的时候，发现有点不对劲。这根本显示不完全啊。

![](https://i.loli.net/2018/04/02/5ac219316f9fd.jpg)


于是，我打开了 404 网站，查询得知。

```
在 Android 的列表嵌套中，第二层的列表无法正常的计算出高度，所以，只有显示一个子项目的高度
```

起初，我还很不服气，觉得就应该使用两层列表嵌套，查询了很多方案，最终发现网上的那么什么重写 ``onMeasure()` 根本没有什么作用；动态添加 `LinearLayout` ，然而会多次重复调用 `getView()` 方法，导致性能下降。

## 2. 使用一层 RecycleView 解决

于是，在一段时间段的焦头烂额的寻找解决方案的时候，我灵机一动，发现有一种方案可行。

```
使用一层 RecycleView 是可行的，可以将每个子项目都带有年份，假如是该年份的第一次项目的话，就显示出来，如果不是的话，就隐藏起来不就能实现了吗？
```

### 2.1 XML 布局

在 XML 布局中，父布局是 `LinearLayout`，然后在其内部分为两部分：

* TextView：用来显示年份
* LinearLayout：用来显示具体时间和事件
    * TextView: 用来显示具体时间
    * TextView: 用来显示具体事件

话不多说，看代码：

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:orientation="vertical">

     <!--用来显示年份，默认的 visibility 为 Gone-->
    <TextView
        android:id="@+id/year_of_group_memory"
        android:layout_width="wrap_content"
        android:layout_height="0dp"
        android:layout_weight="2"
        android:textSize="36sp"
        android:layout_marginLeft="20dp"
        android:visibility="gone" />
    
    <!--用来显示第二层子项目-->
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:layout_marginTop="20dp"
        android:orientation="horizontal"
        android:layout_weight="1">

        <TextView
            android:layout_width="0dp"
            android:layout_height="match_parent"
            android:layout_weight="1"
            android:layout_marginLeft="10dp"
            android:layout_marginRight="10dp"/>

        <TextView
            android:id="@+id/month_and_day_of_memory"
            android:layout_width="0dp"
            android:layout_weight="3"
            android:layout_marginLeft="10dp"
            android:layout_height="match_parent"
            android:textSize="18sp" />

        <TextView
            android:id="@+id/content_of_memory"
            android:layout_width="0dp"
            android:layout_weight="6"
            android:layout_height="match_parent"
            android:textSize="18sp"/>
    </LinearLayout>
</LinearLayout>
```

这样逻辑就很明了了。

1. 假如子项目是在该年份下的第一个项目，id 为 `year_of_group_memory` 的 TextView 显示出来。
2. 假如子项目不是该年份下的第一个项目，id 为 `year_of_group_memory` 的 TextView 就隐藏。

### 2.2 Adapter 代码

布局文件写好了，改写适配代码了。首先，我们的数据类是这样的。

```kotlin
package me.chiaohou.love.bean

import java.sql.Timestamp

data class MemoriesBean(val timeOfMemory: Timestamp, val contentOfMemory: String) : Comparable<MemoriesBean> {

    /**
     * 按照 timestamp 来实现降序排序
     */
    override fun compareTo(other: MemoriesBean): Int {
        return when {
            other.timeOfMemory < this.timeOfMemory -> -1
            other.timeOfMemory > this.timeOfMemory -> 1
            else -> 0
        }
    }

    override fun toString(): String {
        return "MemoriesBean(timeOfMemory=$timeOfMemory, contentOfMemory='$contentOfMemory')"
    }
}
```

可以看出，只有两个属性，分别用来表示时间和具体时间。然后实现了 `Comparable` 接口，用来进行所有的时间排序。

那如何具体的实现分组呢？我的逻辑是这样的，在适配器类中定义一个 `HashSet`，用来存储年份是否已经显示过。

* 得到每次传入项目的 timestamp 属性，得到年份。
* 假如在 HashSet 中没有存在该年份，则在布局中将表示年份的 TextView 设置为 Visible。
* 假如在 HashSet 中存在有该年份，则不进行有关年份控件的操作。

具体代码如下：

```kotlin
override fun onBindViewHolder(holder: ViewHolder, position: Int) {
    val memory = memories[position]

    /**
     * 需要对 memories 进行分组，按照年份分组
     *
     */
    val yearOfMemory = SimpleDateFormat("yyyy").format(memory.timeOfMemory)
    // 如果还没有显示年份，则显示
    if (!yearSet.contains(yearOfMemory)) {
        holder.yearOfMemories.visibility = View.VISIBLE
        holder.yearOfMemories.text = yearOfMemory
        yearSet.add(yearOfMemory)
    }else{
        holder.yearOfMemories.visibility = View.GONE
    }

    // 时间显示例子：03 - 21
    holder.monthAndDayOfMemory.text = SimpleDateFormat("MM - dd").format(memory.timeOfMemory)
    holder.contentOfMemory.text = memory.contentOfMemory
}
```

当一切就绪的时候，运行，结果如上图所示，表示成功了。

## 3 总结

* Android 项目中不建议使用列表嵌套

如果有更好的解决方案，希望能够[交流](keatingsmitht@gmail.com)

