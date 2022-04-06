# 自动Focus的输入框

1. 对设置TextInputEditText：

```
android:focusable="true"
android:focusableInTouchMode="true"
```

2. 对Activity设置

```
android:windowSoftInputMode="stateVisible|adjustPan"
```

3. 在代码中找到该组件，延时100ms弹窗，立即弹窗无效

```
EditText editText = view.findViewById(R.id.autoInput);
        editText.requestFocus();
        InputMethodManager imm = (InputMethodManager) getContext().getSystemService(Context.INPUT_METHOD_SERVICE);
        imm.toggleSoftInput(0, InputMethodManager.SHOW_FORCED);
```
