# Android检查软键盘弹出关闭

原理：检查一个固定位置的View的位置变化。 如重写View 的`onLayout`方法，用 `View.getTop` 两次top的差值。

###条件限制：

Activity AndroidManifest文件配置
`android:windowSoftInputMode="adjustResize"`

下面的代码只适用于RelativeLayout，且布局位于bottom

```java
public class SoftKeyboardDetectView extends View {

    static int TOP = 0;

    private static final int MIN_D = ViewUtils.dp2Px(80);

    public SoftKeyboardDetectView(Context context) {
        super(context);
    }

    public SoftKeyboardDetectView(Context context, AttributeSet attrs) {
        super(context, attrs);
    }

    public SoftKeyboardDetectView(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
    }

    @Override
    protected void onLayout(boolean changed, int left, int top, int right, int bottom) {
        super.onLayout(changed, left, top, right, bottom);
        if (top <= 0) {
            return;
        }

        if (TOP == 0) {
            TOP = top;
        }

        if (null != mListener) {
            mListener.onVisible(TOP - top > MIN_D);
        }
    }

    public void setListener(OnKeyboardVisibleListener mListener) {
        this.mListener = mListener;
    }

    private OnKeyboardVisibleListener mListener;

    public interface OnKeyboardVisibleListener {

        void onVisible(boolean visible);
    }
}

```

```xml

<android.app.test.widget.SoftKeyboardDetectView
        android:id="@+id/vD"
        android:layout_width="1px"
        android:layout_height="1px"
        android:layout_alignParentBottom="true"/>
```

### 使用时注意：

设置Listener的时候最好能delay 1秒，否则在页面刚渲染完成就会回调`onVisible`

```
view.postDelayed(new Runnable() {
            @Override
            public void run() {
                detect();
            }
        }, 1000);

private void detect() {
        view.setListener(new SoftKeyboardDetectView.OnKeyboardVisibleListener() {
            @Override
            public void onVisible(boolean visible) {
                Toast.makeText(KeyboardVisibleActivity.this, visible ? "Show" : "Hide", Toast.LENGTH_SHORT).show();
            }
        });
    }
```