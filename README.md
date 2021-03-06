# MonthWeekMaterialCalendarView
[![](https://jitpack.io/v/idic779/monthweekmaterialcalendarview.svg)](https://jitpack.io/#idic779/monthweekmaterialcalendarview)
 [![API](https://img.shields.io/badge/API-22%2B-blue.svg?style=flat)](https://android-arsenal.com/api?level=22)

[apk download](https://github.com/idic779/MonthWeekMaterialCalendarView/blob/master/monthweekview.apk)

#### 觉得有帮助的可以给个star,有问题联系 idic779@163.com  QQ 290950778

![水滴效果.gif](http://upload-images.jianshu.io/upload_images/2672721-6efd3e0c7670f44d.gif?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)
![普通切换.gif](http://upload-images.jianshu.io/upload_images/2672721-9554d6936f1390a4.gif?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)

可以点击进去查看实现过程
[纵享丝滑滑动切换的周月日历，可高度定制，仿小米日历](https://juejin.im/post/5a631efd6fb9a01ca8720f80)


  之前开发任务中有涉及到年月日日历的切换效果，由于是需要联动，想到的方向大概有3种，要么通过处理view的touch事件，要么是通过自定义behavior去实现，要么是通过ViewDragHelper这个神器去实现，网上比较多的是通过自定义bahavior去实现，本文使用的是第三种方法，实现的是一个可高度定制自由切换的周月日历视图,提供一种思路去实现页面联动效果。
#### features
* 可以控制是否允许左右滑动，上下滑动，切换年月
* 流畅的上下周月模式切换
* 自定义日历样式
* 基于material-calendarview 这个库实现，可以下载源码根据需求定制效果

#### Usages
* step 1 : 添加依赖
```
gradle
allprojects {
    repositories {
        ......
        maven { url 'https://jitpack.io' }
    }
}

dependencies {
    ......
   compile 'com.github.idic779:monthweekmaterialcalendarview:1.5'
}
```

* step 2: 添加布局

  ``` <com.amy.monthweek.materialcalendarview.MonthWeekMaterialCalendarView
        android:id="@+id/slidelayout"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:layout_below="@+id/linearlayout">
        <com.prolificinteractive.materialcalendarview.MaterialCalendarView
            android:id="@+id/calendarView_month_mode"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            app:mcv_calendarMode="month"
            app:mcv_showOtherDates="other_months"
            app:mcv_showWeekView="false" />
        <com.prolificinteractive.materialcalendarview.MaterialCalendarView
            android:id="@+id/calendarView_week_mode"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:background="@android:color/white"
            android:visibility="invisible"
            app:mcv_calendarMode="week"
            app:mcv_showTopBar="false"
            app:mcv_showWeekView="false" />
        <android.support.v7.widget.RecyclerView
            android:id="@+id/recyclerView"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:background="@android:color/white"
            android:orientation="vertical" />
        <LinearLayout
            android:id="@+id/weekview_top"
            android:layout_width="match_parent"
            android:layout_height="44dp"
            android:background="@color/colorPrimary"
            android:orientation="horizontal">
            <TextView
                android:layout_width="0dp"
                android:layout_height="match_parent"
                android:layout_weight="1"
                android:gravity="center"
                android:text="周日" />
            <TextView
                android:layout_width="0dp"
                android:layout_height="match_parent"
                android:layout_weight="1"
                android:gravity="center"
                android:text="周一" />
            <TextView
                android:layout_width="0dp"
                android:layout_height="match_parent"
                android:layout_weight="1"
                android:gravity="center"
                android:text="周二" />
            <TextView
                android:layout_width="0dp"
                android:layout_height="match_parent"
                android:layout_weight="1"
                android:gravity="center"
                android:text="周三" />
            <TextView
                android:layout_width="0dp"
                android:layout_height="match_parent"
                android:layout_weight="1"
                android:gravity="center"
                android:text="周四" />
            <TextView
                android:layout_width="0dp"
                android:layout_height="match_parent"
                android:layout_weight="1"
                android:gravity="center"
                android:text="周五" />
            <TextView
                android:layout_width="0dp"
                android:layout_height="match_parent"
                android:layout_weight="1"
                android:gravity="center"
                android:text="周六" />
        </LinearLayout>
    </com.amy.monthweek.materialcalendarview.MonthWeekMaterialCalendarView>


* step 3: 如何使用

* 设置当前日期，日历才会滚动到对应日期

    monthWeekMaterialCalendarView.setCurrentDate(selectedDate);

* 设置选中日期

    monthWeekMaterialCalendarView.setSelectedDate(selectedDate)
* 添加日历的样式，例如红点 或者自定义图案

    monthWeekMaterialCalendarView.addDecorator(new EventDecorator(Color.RED, dates))
* 移除日历样式

    monthWeekMaterialCalendarView.removeDecorators();
* 设置当前的模式

    monthWeekMaterialCalendarView.setMode(MonthWeekMaterialCalendarView.Mode.MONTH)
* 跳转到上一个月

    monthWeekMaterialCalendarView.goToPrevious();
* 跳转到下个月

    monthWeekMaterialCalendarView.goToNext();
* 设置是否允许竖直拖动，默认是允许拖动切换周月模式

    monthWeekMaterialCalendarView.setCanDrag
* 设置是否允许左右滑动

    monthWeekMaterialCalendarView.setPagingEnabled
* 添加选中日期、模式改变 或者月份改变的回调

```java
    monthWeekMaterialCalendarView.state().edit()
                //设置最大最小日期
                .setMinimumDate(new CalendarDay(2017,1,1))
                .setMaximumDate(new CalendarDay(2018,3,1))
                .setSlideModeChangeListener(new MonthWeekMaterialCalendarView.SlideModeChangeListener() {
            @Override
            public void modeChange(MonthWeekMaterialCalendarView.Mode mode) {

            }
        }).setSlideDateSelectedlistener(new MonthWeekMaterialCalendarView.SlideDateSelectedlistener() {
            @Override
            public void onDateSelected(@NonNull MaterialCalendarView widget, @NonNull CalendarDay date, boolean selected) {

            }
        }).setSlideOnMonthChangedListener(new MonthWeekMaterialCalendarView.SlideOnMonthChangedListener() {
            @Override
            public void onMonthChanged(MaterialCalendarView widget, CalendarDay date) {

            }
        }).commit();
```
* 设置选中颜色
    monthWeekMaterialCalendarView.setSelectionColor
    因为基于MaterialCalendarView的周和月视图组成的，所以可以在XML中设置选中颜色，字体样式等等
```xml
       <com.prolificinteractive.materialcalendarview.MaterialCalendarView
            android:id="@+id/calendarView_month_mode"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            app:mcv_selectionColor="@color/colorPrimary"
            app:mcv_dateTextAppearance="@style/TextAppearance.MaterialCalendarWidget.Date"
            app:mcv_calendarMode="month"
            app:mcv_showOtherDates="defaults|other_months"
            app:mcv_showWeekView="false" />
        <com.prolificinteractive.materialcalendarview.MaterialCalendarView
            android:id="@+id/calendarView_week_mode"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:background="@android:color/white"
            android:visibility="invisible"
            app:mcv_selectionColor="@color/colorPrimary"
            app:mcv_dateTextAppearance="@style/TextAppearance.MaterialCalendarWidget.Date"
            app:mcv_calendarMode="week"
            app:mcv_showTopBar="false"
            app:mcv_showWeekView="false" />
```
# 还可以怎么用

      接下来说下你可以怎么去定制？如果你想替换项目中的月和周视图的话，很简单，

    只需要你自己的周月视图必须有一个方法获得单行日历的高度(例如我的库中的MaterialCalendarView.getItemHeight() )，

    然后把这个月视图和周视图，分别在MonthWeekMaterialCalendarView里面按照顺序放到对应位置即可。

    然后再setListener()里面设置相关的回调处理，例如日期选中或者月份切换的回调等。

    更多的使用方法可以下载demo参考
# thanks
[material-calendarview](https://github.com/prolificinteractive/material-calendarview)

