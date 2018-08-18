# Android Dialog 正确使用姿势

google 官方文档不推荐直接使用Dialog，而是用DialogFragment将Dialog 包起来使用。好处是防止activity被系统回收，而dialog 没关闭的话会导致内存泄漏。除非在Activity onDestroy 里面关闭dialog。很烦，是不是～所以还是用官方推荐的DialogFragment吧，没准哪天google改实现了呢

为了更好的用户体验，DialogFragment 在onRestoreInstanceState的时候会重新创建dialog，不是通过bundle传的参数都会丢失。比如listener，callback之类不能序列化的参数。

怎么解决呢？下面有几种办法：

### 1. 不让 DialogFragment 重建，如果onRestoreInstanceState调用，那就关闭dialog吧！

重写 `DialogFragment#onSaveInstanceState`

```
public class NormalDialog1 extends DialogFragment {

	private DialogInterface.OnClickListener listener;

	@Override
	public void onSaveInstanceState(Bundle outState) {
		setShowsDialog(false);
		super.onSaveInstanceState(outState);
	}
}
```

### 2. 使用静态属性（不推荐）

具体实现就不写了，注意重置值就好

### 3. `Activity` 实现`Callback`或者`Listener`

Activity实现`callback`，需要用listener的时候强转就好了

## 总结

好吧！上面几种都不是很完美的方案，我个人比较偏向第一种，dialog消失没啥关系，用户再点击一次就好了。
系统做的太多反而不好，要管理状态会让app变复杂，bug也有可能增多