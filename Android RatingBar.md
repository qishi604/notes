# Android RatingBar

## xml 布局

```xml
<RatingBar
	android:layout_width="wrap_content"
	android:layout_height="wrap_content"
	android:minHeight="12dp"
	android:numStars="5"
	android:rating="8"
	android:stepSize="0.1"
	android:progressDrawable="@drawable/rating_bar"
	/>
```

## 属性

- android:isIndicator 是否仅用于展示，不能改变进度
- android:numStars 星星数量（浮点值）
- android:rating 初始分数,
- android:stepSize 每一步递增数值

重点解释下`rating`和`stepSize`

`rating`表示分数，其实就是星星的数量。一般星星都是5个，所以最大分数为5，`rating`设置为4.5就4.5个星。

`stepSize`跟`onRatingChanged`事件有关，例如设置为0.1，则每次变化最小为0.1。

## 事件

```
onRatingChanged(RatingBar ratingBar, float rating, boolean fromUser)
```

# 应用

需求：电影最高评分为10，保留一个小数位，如9.8分。

实现

rating_bar.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<layer-list xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:id="@android:id/background" android:drawable="@drawable/off" />
    <item android:id="@android:id/secondaryProgress" android:drawable="@drawable/off" />
    <item android:id="@android:id/progress" android:drawable="@drawable/on" />
</layer-list>
```

```xml
<RatingBar
	android:layout_width="wrap_content"
	android:layout_height="wrap_content"
	android:minHeight="12dp"
	android:numStars="5"
	android:stepSize="0.05"
	android:progressDrawable="@drawable/rating_bar"
	/>
```

``` java
RatingBar rb = (RatingBar) findViewById(R.id.rating);
// 设置初始值8.0分
rb.setRating(4.0f); 
rb.setOnRatingBarChangeListener(new RatingBar.OnRatingBarChangeListener() {
    @Override
    public void onRatingChanged(RatingBar ratingBar, float rating, boolean fromUser) {
       float real rate = rating * 2;
        System.out.println("rating: " + rating);
    }
});
```

效果图：(下面的RatingBar切图没留白)

![效果图](./images/img-rating.png)

## 注意事项
### 切图

星星的间隙是靠切图来留白实现的，所以切图的时候注意留白。如果上下不留白，星星底下两个角会出现细线。

### 高度

RatingBar 默认的style 有一个最小高度，所以最好是设置最小高度等于图片的高度