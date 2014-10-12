# 非同期処理/スレッド

1.  UIスレッドで通信しない

    ネットワーク通信はUIスレッド以外で行なう。  
    Android 4.0(ICS)以降では、UIスレッドで通信するとクラッシュさせられてしまう。

1.  Activity, Service, Fragmentで同期しない

    `Activity`, `Service`, `Fragment`といったライフサイクルを持つクラスの
    インスタンスは状況によりインスタンスが再生成されるため、
    古い参照を使って同期を取ろうとしたりすると失敗する。  
    ハードウェアのロックなど、アプリ内で必ず同期処理する必要のあるものは
    staticなフィールドで同期を取るようにする。

1.  AsyncTaskの並列性の違いを考慮する

    Android 3.0以降のAPI Levelでビルドする場合、
    `AsyncTask`がデフォルトでシリアル実行になることを考慮する。
    パラレル実行が必要な場合は
    `AsyncTask#executeOnExecutor(AsyncTask.THREAD_POOL_EXECUTOR, params)`を使うこと。

    例(パラレル実行したい場合):

    ```java
    if (Build.VERSION.SDK_INT < Build.VERSION_CODES.HONEYCOMB) {
        execute(params);
    } else {
        executeOnExecutor(AsyncTask.THREAD_POOL_EXECUTOR, params);
    }
    ```
