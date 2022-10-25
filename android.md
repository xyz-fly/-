### android:fitsSystemWindows
设置了该属性的view的所有padding属性将失效，系统会根据情况给该view添加一个值等于状态栏高度的paddingTop，一个值等于导航栏高度的paddingBottom。
但不是所有外层布局都支持，FrameLayout就不支持，而CoordinatorLayout则支持，是它里面处理了fitsSystemWindows设置

```
    private void setupForInsets() {
        if (Build.VERSION.SDK_INT < 21) {
            return;
        }

        if (ViewCompat.getFitsSystemWindows(this)) {
            if (mApplyWindowInsetsListener == null) {
                mApplyWindowInsetsListener =
                        new androidx.core.view.OnApplyWindowInsetsListener() {
                            @Override
                            public WindowInsetsCompat onApplyWindowInsets(View v,
                                    WindowInsetsCompat insets) {
                                return setWindowInsets(insets);
                            }
                        };
            }
            // First apply the insets listener
            ViewCompat.setOnApplyWindowInsetsListener(this, mApplyWindowInsetsListener);

            // Now set the sys ui flags to enable us to lay out in the window insets
            setSystemUiVisibility(View.SYSTEM_UI_FLAG_LAYOUT_STABLE
                    | View.SYSTEM_UI_FLAG_LAYOUT_FULLSCREEN);
        } else {
            ViewCompat.setOnApplyWindowInsetsListener(this, null);
        }
    }
```

关键是setSystemUiVisibility(View.SYSTEM_UI_FLAG_LAYOUT_STABLE | View.SYSTEM_UI_FLAG_LAYOUT_FULLSCREEN);  
具体文章参见：https://blog.csdn.net/guolin_blog/article/details/123023395
