# ToastUtil

目录：

- [ToastUtil](#toastutil)

## ToastUtil

ToastUtil.java

```
import android.annotation.SuppressLint;
import android.content.Context;
import android.widget.Toast;

public class ToastUtil {
    private static Toast sToast;
    @SuppressLint("ShowToast")
    public static void showToast(Context context, String content){
        if(sToast==null){
            sToast=Toast.makeText(context,content,Toast.LENGTH_SHORT);
        }else {
            sToast.setText(content);
        }
        sToast.show();
    }
}
```
这是一个Toast公共类。

即使触发多次，也只会持续一个Toast显示的时间，如果是在触发Toast还没结束的情况下继续触发其他的Toast，则只会修改当前显示的Toast的内容。