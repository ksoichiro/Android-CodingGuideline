# コールバック

1.  内部クラス、内部インタフェースの参照

    `View.OnClickListener`と`DialogInterface.OnClickListener`など、
    特にリスナーなどのクラス名・インタフェース名は他のパッケージやクラスの
    名前と重複しやすい。
    そのため、importせずに包含クラス名から記述することで、読み手の誤解をなくす。

    悪い例:

    ```java
    import android.view.View;
    import View.OnClickListener;
    :
    findViewById(R.id.button).setOnClickListener(
            // どのクラスに属するインタフェースを使っているのか読み取りにくい
            new OnClickListener() {
                @Override
                public void onClick(View v) {
                }
            });
    ```

    良い例:

    ```java
    import android.view.View;
    :
    findViewById(R.id.button).setOnClickListener(
            // Viewクラスのインタフェースを使っているのが明らか
            new View.OnClickListener() {
                @Override
                public void onClick(View v) {
                }
            });
    ```

1.  コールバックメソッドのシグニチャの衝突を避ける

    非同期処理の完了イベントハンドラ等、新たにコールバックインタフェースを設けることは
    頻繁に発生するが、単純な名前にしてしまうと名前が衝突してしまう。
    一意になりやすい名前をつけるか、パラメータにコールバック元の
    オブジェクトを含めることで回避する。
    
    悪い例:
    
    ```java
    // onComplete()のシグニチャが同一なため
    // 両方のインタフェースを同じクラスで実装することができない
    class LoginTask extends AsyncTask {
        interface OnCompleteListener {
            onComplete();
        }
    }
    class LogoutTask extends AsynkTask {
        interface OnCompleteListener {
            onComplete();
        }
    }
    ```
    
    良い例:
    
    ```java
    // 名前で衝突を避ける
    class LoginTask extends AsyncTask {
        interface LoginListener {
            onLoginComplete();
        }
    }
    class LogoutTask extends AsyncTask {
        interface LogoutListener {
            onLogoutComplete();
        }
    }
    
    // パラメータで衝突を避ける
    class LoginTask extends AsyncTask {
        interface OnCompleteListener {
            onComplete(LoginTask task);
        }
    }
    class LogoutTask extends AsyncTask {
        interface OnCompleteListener {
            onComplete(LogoutTask task);
        }
    }
    ```

