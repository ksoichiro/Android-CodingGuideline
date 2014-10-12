# 命名規則

1.  クラス名

    先頭と区切りを大文字に、ハイフンは使わない。

    ```java
    CapitalizedWithInternalWordsAlsoCapitalized
    ```

1.  例外クラス名

    最後をExceptionとしたクラス名。

    ```java
    ClassNameEndsWithException
    ```

1.  インタフェース名

    クラス名と同様。

    ```java
    NameOfInterface
    ```

1.  実装クラス名

    特に`interface`と区別の必要があれば、最後にImplを付ける。

    ```java
    ClassNameEndsWithImpl
    ```

1.  抽象クラス名

    抽象クラス名に適当な名前が無いとき、`Abstract`あるいは`Base`から始まり
    サブクラス名を連想させる名前を付ける。

    ```java
    AbstractAsyncTask
    BaseActivity
    ```

1.  定数(static final)

    大文字を"_"でつなぐ。

    ```java
    UPPER_CASE_WITH_UNDERSCORES
    ```

1.  メソッド名

    最初小文字で、あとは区切りを大文字。

    ```java
    firstWordLowerCaseButInternalWordsCapitalized()
    ```

1.  ファクトリメソッド

    ```java
    X newX()
    X createX()
    ```

1.  コンバータメソッド

    ```java
    X toX()
    ```

1.  属性の取得メソッド

    ```java
    X x()
    X getX()
    boolean isEnabled()
    ```

1.  属性の設定メソッド

    ```java
    void setX(X value)
    ```

1.  boolean変数を返すメソッド

    is + 形容詞、can + 動詞、has + 過去分詞、should + 動詞、三単元動詞、三単元動詞 + 名詞。

    ```java
    boolean isEmpty()
    boolean canGet()
    boolean hasChanged()
    boolean contains(Object)
    boolean containsKey(Key)
    boolean shouldStartLoading()
    ```

    理由:

    * if, while文等の条件が読みやすくなる。またtrueがどちらの意味か分かりやすい。

1.  boolean変数

    形容詞、is + 形容詞、can + 動詞、has + 過去分詞、三単元動詞、三単元動詞 + 名詞。

    ```java
    boolean isEmpty
    boolean dirty
    boolean containsMoreElements
    ```

1.  英語と日本語

    すべての識別子の名前は英語を基本とするが、
    外部インタフェースでのネーミングと合わせる目的や、
    適切な英語がない(無理に変換すると誤解を招く場合)は日本語(ローマ字表記)でも良い。

1.  名前の対称性

    クラス名、メソッド名を付ける際は、以下の英語の対称性に気を付ける。  
    下記は参考例。  
    スーパークラスや既存のメソッドに同種のメソッドがある場合は、それらの命名にならうこと。

    ```
    add/remove
    insert/delete
    get/set
    get/release
    start/stop
    start/finish
    begin/end
    send/receive
    first/last
    put/get
    push/pop
    up/down
    show/hide
    source/target
    open/close
    source/destination
    increment/decrement
    lock/unlock
    old/new
    next/previous
    ```

1.  ループカウンタ

    スコープ（通用範囲）が狭いループカウンタ、イテレータにi, j, k という名前をこの順に使う。  
    原則として、カウンタを必要としないループでは、拡張for文を使う。

    理由:

    * ループカウンタを間違ってしまっても、コンパイル時にはエラーが
      起こらず不具合を起こしやすい。  
      ルール付けをしておくことで少しでもミスを低減する。

    * 単に順番に要素を取得するだけの処理ならば、
      拡張for文を使うことでループカウンタの利用そのものをなくすことができる。

1.  スコープが狭い名前

    スコープが狭い変数名は、型名を略したものを使って良い。  

    例:

    ```java
    Resources res = getResources();
    ```

1.  意味がとれる名前

    変数名から役割が読み取れる名前にする。

    悪い例:

    ```java
    copy(s1, s2)
    ```java

    良い例:

    ```java
    copy(from, to) あるいはcopy(source, destination)
    ```

1.  無意味な名前

    Info, Data, Temp, Str, Bufという名前は再考を要する。  

    悪い例:

    ```java
    double temp = Math.sqrt(b*b - 4*a*c);
    ```

    良い例:

    ```java
    double determinant = Math.sqrt(b*b - 4*a*c);
    ```

1.  大文字小文字

    大文字と小文字は別な文字として扱われるが、
    それのみで区別される名前を付けてはならない。

1.  フィールド名は頭文字をmにする

    AOSP(Android Open Source Project)でのルールに従い、
    フィールド名は`mName`のように先頭m + 先頭大文字の単語を連結とする。  
    ただしフォーム、エンティティ等のフィールドのみの簡単なクラスは例外とする。  
    また、ローカル変数や定数など、他の種類の変数ではこのルールを適用しないこと。

    良い例:

    ```java
    private Context mContext;
    ```

    悪い例:

    ```java
    // 定数には適用しない
    public static final String mArgName = "name";
    private void doSomething() {
        int mValue; // ローカル変数には適用しない
    }
    // getmContextとしない
    protected Context getContext {
        return mContext;
    }
    ```

1.  XMLのネーミングはAOSPやADTのジェネレータに合わせる

    MainActivityのレイアウト：

    ```java
    res/layout/activity_main.xml
    ```

    MainFragmentのレイアウト：

    ```java
    res/layout/fragment_main.xml
    ```

    文字列：  

    ```java
    res/values/strings.xml
    ```

    大量にある場合は、文字列の種類ごとにファイルを分けても良い。  
    例えば、利用ソフトウェアのライセンス文言を`strings_license.xml`などとして分離する。

    配列：

    ```java
    res/values/arrays.xml
    ```

    色：

    ```java
    res/values/colors.xml
    ```

    サイズ：

    ```java
    res/values/dimens.xml
    ```

    スタイル：

    ```java
    res/values/styles.xml
    ```


1.  外部システム上の名前に対応する名前は完全一致させる

    例えば、外部システムのAPIに渡すパラメータ名`p1`に関連する
    定数として`P1_CODE`というような
    微妙に差異のある名前をつけると不具合に繋がるので避ける。

1.  既存のネーミングルールを破壊する名前を付けない

    他システムでのID等、何らかのネーミングルールがある名前に類似した、
    意味のない名前を付けないこと。  

    例:  
    システムAのAPI001, 002へのリクエストクラスのネーミングとして

    ```java
    A001Request
    A002Request
    ```

    が定義されている状況で、全く関連のない他システムへのリクエストに対して

    ```java
    A999Request
    ```

    などとネーミングするのはNG。

1.  boolean型の戻り値を返すメソッド名は判定している内容と合うようにする

    エラーチェックの結果、チェック失敗の場合にエラーメッセージを出す、
    という条件文を書きたいがために
    チェックメソッド名にSuccessという単語を使うのは、
    実際の処理内容と逆の意味になるため避けること。

    悪い例: 

    ```java
    boolean inputCheckSuccess(String s) {
        // 必須チェック
        return TextUtils.isEmpty(s);
    }
    :
    if (inputCheckSuccess(s)) {
        // 入力は成功しているのにエラーメッセージ？
        // 誤解を招きやすいネーミング
        Toast.makeText(this, "XXXは必須です",
                Toast.LENGTH_SHORT).show();
    }
    ```

    良い例:

    ```java
    boolean inputHasError(String s) {
        // 必須チェック
        return s == null;
    }
    :
    if (inputHasError(s)) {
        Toast.makeText(this, "XXXは必須です",
                Toast.LENGTH_SHORT).show();
    }
    ```

1.  XMLリソース名にJavaの予約語を使わない

    XMLリソースでの`id`等は`R.java`に変換されJava言語上の識別子となるため、
    `main`や`import`のような予約語を使わないこと。
