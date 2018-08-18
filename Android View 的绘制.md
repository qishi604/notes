# Android View 的绘制

## 源码位置

`android.view.ViewRootImpl` 负责View 的绘制
`com.android.internal.policy.DecorView` Window 的顶层View
`com.android.internal.policy.PhoneWindow` Activity 的 Window 实现类
`android.app.ActivityThread` 负责创建 Activity 并处理生命周期

## View 的绘制起点

```java
@Override
    public void requestLayout() {
        if (!mHandlingLayoutInLayoutRequest) {
            checkThread();
            mLayoutRequested = true;
            // 这个方法会向主线程发送一个“遍历”消息，后面会调用 performTraversals() 
            scheduleTraversals();
        }
    }
```

## View 绘制分三个阶段

- measure 判断是否需要重新计算 View 的大小，需要的话则计算
- layout 判断是否需要重新计算 View 的位置，需要的话则计算
- draw 判断是非需要重新绘制 View ，需要的话则重绘

### measure

计算出控件树中各个控件需要多大尺寸。起点是 ViewRootImpl 的 measureHierarchy 方法

```java
private boolean measureHierarchy(final View host, final WindowManager.LayoutParams lp,
            final Resources res, final int desiredWindowWidth, final int desiredWindowHeight) {
        int childWidthMeasureSpec;
        int childHeightMeasureSpec;
        boolean windowSizeMayChange = false;

        boolean goodMeasure = false;
        if (lp.width == ViewGroup.LayoutParams.WRAP_CONTENT) {
            // On large screens, we don't want to allow dialogs to just
            // stretch to fill the entire width of the screen to display
            // one line of text.  First try doing the layout at a smaller
            // size to see if it will fit.
            final DisplayMetrics packageMetrics = res.getDisplayMetrics();
            res.getValue(com.android.internal.R.dimen.config_prefDialogWidth, mTmpValue, true);
            int baseSize = 0;
            if (mTmpValue.type == TypedValue.TYPE_DIMENSION) {
                baseSize = (int)mTmpValue.getDimension(packageMetrics);
            }

            if (baseSize != 0 && desiredWindowWidth > baseSize) {
                childWidthMeasureSpec = getRootMeasureSpec(baseSize, lp.width);
                childHeightMeasureSpec = getRootMeasureSpec(desiredWindowHeight, lp.height);
                // 执行 measure
                performMeasure(childWidthMeasureSpec, childHeightMeasureSpec);

                if ((host.getMeasuredWidthAndState()&View.MEASURED_STATE_TOO_SMALL) == 0) {
                    goodMeasure = true;
                } else {
                    // Didn't fit in that size... try expanding a bit.
                    baseSize = (baseSize+desiredWindowWidth)/2;
                    
                    childWidthMeasureSpec = getRootMeasureSpec(baseSize, lp.width);
                    
                    performMeasure(childWidthMeasureSpec, childHeightMeasureSpec);
                    if ((host.getMeasuredWidthAndState()&View.MEASURED_STATE_TOO_SMALL) == 0) {
                        goodMeasure = true;
                    }
                }
            }
        }

        if (!goodMeasure) {
            childWidthMeasureSpec = getRootMeasureSpec(desiredWindowWidth, lp.width);
            childHeightMeasureSpec = getRootMeasureSpec(desiredWindowHeight, lp.height);
            performMeasure(childWidthMeasureSpec, childHeightMeasureSpec);
            if (mWidth != host.getMeasuredWidth() || mHeight != host.getMeasuredHeight()) {
                windowSizeMayChange = true;
            }
        }

        return windowSizeMayChange;
    }
```

简单地介绍下MeasureSpec的概念。
`MeasureSpec`是一个32位整数，由`SpecMode`和`SpecSize`两部分组成，其中，高2位为`SpecMode`，低30位为`SpecSize`。`SpecMode`为测量模式，`SpecSize`为相应测量模式下的测量尺寸。View（包括普通View和ViewGroup）的`SpecMode`由本View的`LayoutParams`结合父View的`MeasureSpec`生成。
`SpecMode`的取值可为以下三种：

- `EXACTLY`: 对子View提出了一个确切的建议尺寸（SpecSize）；
- `AT_MOST`: 子View的大小不得超过SpecSize；
- `UNSPECIFIED`: 对子View的尺寸不作限制，通常用于系统内部。

```java

private void performMeasure(int childWidthMeasureSpec, int childHeightMeasureSpec) {
       ...
        try {
            mView.measure(childWidthMeasureSpec, childHeightMeasureSpec);
        } finally {
            Trace.traceEnd(Trace.TRACE_TAG_VIEW);
        }
    }
```
`mView` 即为 decorView，也就是说会调用 `View.measure`

ViewGroup及其子类来说，要先完成子View的测量，再进行自身的测量（考虑进padding等）。

## layout

layout阶段的基本思想也是由根View开始，递归地完成整个控件树的布局（layout）工作。

### View.layout()

```java

public void layout(int l, int t, int r, int b) {
    // l为本View左边缘与父View左边缘的距离
    // t为本View上边缘与父View上边缘的距离
    // r为本View右边缘与父View左边缘的距离
    // b为本View下边缘与父View上边缘的距离
    . . .
    boolean changed = isLayoutModeOptical(mParent) ? setOpticalFrame(l, t, r, b) : setFrame(l, t, r, b);
    if (changed || (mPrivateFlags & PFLAG_LAYOUT_REQUIRED) == PFLAG_LAYOUT_REQUIRED) {
        onLayout(changed, l, t, r, b);
        . . .

    }
    . . .
}
```

这个方法会调用setFrame()方法来设置View的mLeft、mTop、mRight和mBottom四个参数，这四个参数描述了View相对其父View的位置（分别赋值为l, t, r, b），在setFrame()方法中会判断View的位置是否发生了改变，若发生了改变，则需要对子View进行重新布局，对子View的局部是通过onLayout()方法实现了。由于普通View（ 非ViewGroup）不含子View，所以View类的onLayout()方法为空。

### ViewGroup.onLayout()

实际上ViewGroup类的onLayout()方法是abstract，这是因为不同的布局管理器有着不同的布局方式。
这里我们以decorView，也就是FrameLayout的onLayout()方法为例，分析ViewGroup的布局过程：

```java
@Override
protected void onLayout(boolean changed, int left, int top, int right, int bottom) {
  layoutChildren(left, top, right, bottom, false /* no force left gravity */);
}

void layoutChildren(int left, int top, int right, int bottom, boolean forceLeftGravity) {
  final int count = getChildCount();
  final int parentLeft = getPaddingLeftWithForeground();
  final int parentRight = right - left - getPaddingRightWithForeground();
  final int parentTop = getPaddingTopWithForeground();
  final int parentBottom = bottom - top - getPaddingBottomWithForeground();

  for (int i = 0; i < count; i++) {
    final View child = getChildAt(i);
    if (child.getVisibility() != GONE) {
      final LayoutParams lp = (LayoutParams) child.getLayoutParams();
      final int width = child.getMeasuredWidth();
      final int height = child.getMeasuredHeight();
      int childLeft;
      int childTop;
      int gravity = lp.gravity;

      if (gravity == -1) {
        gravity = DEFAULT_CHILD_GRAVITY;
      }

      final int layoutDirection = getLayoutDirection();
      final int absoluteGravity = Gravity.getAbsoluteGravity(gravity, layoutDirection);
      final int verticalGravity = gravity & Gravity.VERTICAL_GRAVITY_MASK;

      switch (absoluteGravity & Gravity.HORIZONTAL_GRAVITY_MASK) {
        case Gravity.CENTER_HORIZONTAL:
          childLeft = parentLeft + (parentRight - parentLeft - width) / 2 +
lp.leftMargin - lp.rightMargin;
          break;

        case Gravity.RIGHT:
          if (!forceLeftGravity) {
            childLeft = parentRight - width - lp.rightMargin;
            break;
          }

        case Gravity.LEFT:
        default:
          childLeft = parentLeft + lp.leftMargin;

      }

      switch (verticalGravity) {
        case Gravity.TOP:
          childTop = parentTop + lp.topMargin;
          break;

        case Gravity.CENTER_VERTICAL:
          childTop = parentTop + (parentBottom - parentTop - height) / 2 +
lp.topMargin - lp.bottomMargin;
          break;

        case Gravity.BOTTOM:
          childTop = parentBottom - height - lp.bottomMargin;
          break;

        default:
          childTop = parentTop + lp.topMargin;
      }
      child.layout(childLeft, childTop, childLeft + width, childTop + height);
    }
  }
}
```

最后会调用child.layout()方法对子View的位置参数进行设置，这时便转到了View.layout()方法的调用，若子View是ViewGroup，则会递归地对其子View进行布局。

## draw 阶段

```java
public void draw(Canvas canvas) {
  . . . 
  // 绘制背景，只有dirtyOpaque为false时才进行绘制，下同
  int saveCount;
  if (!dirtyOpaque) {
    drawBackground(canvas);
  }

  . . . 

  // 绘制自身内容
  if (!dirtyOpaque) onDraw(canvas);

  // 绘制子View
  dispatchDraw(canvas);

   . . .
  // 绘制滚动条等
  onDrawForeground(canvas);

}
```

View类的onDraw()方法为空，因为每个View绘制自身的方式都不尽相同，对于decorView来说，由于它是容器View，所以它本身并没有什么要绘制的。dispatchDraw()方法用于绘制子View，显然普通View（非ViewGroup）并不能包含子View，所以View类中这个方法的实现为空。

ViewGroup类的dispatchDraw()方法中会依次调用drawChild()方法来绘制子View，drawChild()方法的源码如下

```java
protected boolean drawChild(Canvas canvas, View child, long drawingTime) {
  return child.draw(canvas, this, drawingTime);
}
```

这个方法调用了View.draw(Canvas, ViewGroup，long)方法来对子View进行绘制。在draw(Canvas, ViewGroup, long)方法中，首先对canvas进行了一系列变换，以变换到将要被绘制的View的坐标系下。完成对canvas的变换后，便会调用View.draw(Canvas)方法进行实际的绘制工作，此时传入的canvas为经过变换的，在将被绘制View的坐标系下的canvas。

进入到View.draw(Canvas)方法后，会向之前介绍的一样，执行以下几步：

- 绘制背景;
- 通过onDraw()绘制自身内容;
- 通过dispatchDraw()绘制子View;
- 绘制滚动条


## 参考

- [深入理解Android之View的绘制流程](https://zhuanlan.zhihu.com/p/23507232)