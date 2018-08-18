# Bug list

WebView#evaluateJavascript() ANR

一定要注意下面的代码，如果getwith一直为0，会一直调用，导致页面显示不出来～
找了差不多一天啊～～～～

```
mIvDiskControl.getViewTreeObserver().addOnPreDrawListener(new ViewTreeObserver.OnPreDrawListener() {
			@Override
			public boolean onPreDraw() {
				if (mIvDiskControl.getWidth() > 10) {
					mIvDiskControl.getViewTreeObserver().removeOnPreDrawListener(this);
					mIvDiskControl.setPivotX(mIvDiskControl.getWidth());
					mIvDiskControl.setPivotY(mIvDiskControl.getHeight());
					mIvDiskControl.setRotation(-25);
					return true;
				}
				return false;
			}
		});
```