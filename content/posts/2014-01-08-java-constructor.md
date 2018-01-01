---
layout: post
title: 'Java constructor'
date: 2014-01-08 09:59
comments: true
categories: 
---
純粹是驗證，雖然已經知道結果，還是動手確認一下。

```
// Activity.java
class Activity extends Context {
}
```

```
// BrowserActivity.java
class BrowserActivity extends Activity {
    BrowserActivity() {
        Controller controller = new Controller(this);
    }
}
```

```
// Context.java
public class Context {
}
```

```
// Controller.java
import static java.lang.System.*;
class Controller {
    Controller(Context ctx) {
        out.println("Context");
    }
    Controller(Activity act) {
        out.println("Activity");
    }
}
```

```
// Hello.java 
// 這是主程式
import static java.lang.System.*;

public class Hello {
    public static void main(String[] args) {
        out.println("Hello world");
        out.println("=====");
        BrowserActivity activity = new BrowserActivity();
        out.println("=====");
    }
}
```

執行結果：
```
Hello world
=====
Activity
=====
```

當然是會執行到 ctor 參數為 Activity 的那個。