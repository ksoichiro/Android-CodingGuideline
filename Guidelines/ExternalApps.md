# 外部アプリとの連携

1.  Intentが存在するかチェックする

    いきなり`Activity#startActivity()`を呼び出さず、`PackageManager#queryIntentActivities()`でチェックしてから呼び出す。

    理由:

    * 端末(ユーザプロファイル)によっては、期待するアプリ(アクティビティ)にアクセスできない場合もある。そのような場合にチェックせずに呼び出すと、`ActivityNotFoundException`が発生してクラッシュしてしまう。

    例:

    ```java
    Intent intent = new Intent(Intent.ACTION_VIEW);
    intent.setData(Uri.parse("market://details?id=xxxxxxxxxx"));
    if (0 < getPackageManager().queryIntentActivities(intent, 0).size()) {
        startActivity(intent);
    } else {
        Toast.makeText(this, "Playストアにアクセスできませんでした", Toast.LENGTH_SHORT).show();
    }
    ```
