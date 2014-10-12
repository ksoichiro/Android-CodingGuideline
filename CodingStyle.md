# コーディングスタイル

1.  インデント

    半角スペース4文字を1インデントとする。

1.  位置調整の余白は使わない

    フォーマットのメンテナンスはIDEのフォーマッタ機能を使うため、
    原則として手作業で余白を挿入しての位置調整を行わないこと。

1. ブロック

    ブロックの括弧は末尾に付ける。
    
    例:
    
    ```java
    // OK
    if (A) {
    }
    
    // NG
    if (A)
    {
    }
    
    // OK
    class A {
    }
    
    // NG
    class A
    {
    }
    ```

1.  スペース

    区切り部分には半角スペースを入れる。  
    既存プロジェクトへの変更ではプロジェクトのルールに従う。  
    AOSPのソースコードの慣例も参考にする。

    例:

    ```java
    // ifの後、ブロックの開始前にはスペース、式の前後にはスペースなし
    if (map.contains(key)) { // OK
    if( map.contains( key ) ) { // NG

    // elseの前後にはスペース
    } else { // OK
    }else{ // NG

    // コメント記号の後にはスペース
    //NG
    // OK

    // メソッド名の後にはスペースなし、ブロックの開始前にはスペース
    public void function(int a, String b) { // OK
    public void function ( int a, String b ){ // NG

    // キャストの後にはスペース
    EditText e = (EditText)findViewById(R.id.name); // NG
    EditText e = (EditText) findViewById(R.id.name); // OK

    // 演算子の前後にはスペース
    int x=a*2; // NG
    int y = a * 2; // OK
    ```

1.  行の最大文字数

    原則として一行は最大80桁とし、それを超える場合は行を分割する。  
    基本的にIDEを使用して自動整形すること。

1.  1文のみのブロックでも括弧で括る

    処理が1文しかないif文など、括弧を書かなくても動作する処理であっても
    括弧はつけておく。

    理由:

    * if文内に処理を追加した場合に、
      if文の外に出てしまったことに気付かず、バグになる可能性がある。

    悪い例:

    ```java
    if (condition)
        doSomething(1);
    
    if (condition)
        doSomething(1);
        doSomething(2); // これはcondition == falseでも実行されるが見落としやすい
    ```

    良い例:

    ```java
    if (condition) {
        doSomething(1);
        doSomething(2); // これはcondition == trueでのみ実行されることが明らか
    }
    ```

1.  深い階層を避ける

    OSの特性上、イベントリスナーを多用するスタイルになるため、階層が深くなりがちである。
    if-else, try-catchなどの深い階層は可能な限り避ける。

    悪い例:

    ```java
    public void doSomething(List<Data> list) {
        if (list != null && list.size() > 0) {
            // elseの分岐がないのにifに処理を書くと無駄に階層が深くなる
            for (Data data : list) {
                :
            }
        }
    }
    ```

    良い例:

    ```java
    public void doSomething(List<Data> list) {
        if (list == null || list.size() == 0) {
            return;
        }
        // 異常値を先に弾くことで正常処理の階層を浅くする
        for (Data data : list) {
            :
        }
    }
    ```

1.  配列の最終要素にもカンマを付ける

    例:
    
    ```java
    private static final int[] codes = new int[] {
        1,
        2,
        3,
    };
    ```

    理由:
    
    * 新しく要素を付け加えるときに最終要素の行を編集しなくて済む
