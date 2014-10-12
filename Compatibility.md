# OSバージョン互換性/複数OSバージョンへの対応

1.  Android 3.0以降ではHoloテーマを使う

    Android 3.0(Honeycomb)以降では、`Holo`テーマが導入され、
    アプリのUIイメージが大きく変わっている。  
    このため、Android 3.0前後をサポートする場合は
    スタイルをAndroid 3.0以降とそれ以前で分けて、
    Android 3.0以降のスタイルは`Holo`テーマをベースにすること。  
    同様にAndroid 4.0ではHoloスタイルがさらに充実し、
    Android 4.4(KitKat)でも一部のデザインが変わっているため
    必要に応じて切り替える。

    理由:

    * エンドユーザが最も馴染みのあるのは利用しているOSバージョンの標準UIである。
    
    注:

    * 要件として使用しないことが定まっている場合はこの限りではない。

1.  リソースを分ける

    OSバージョン等で必要に応じてレイアウトファイルを分ける。  
    例えば、`res/layout/sample.xml`と`res/layout-v11/sample.xml`を用意すれば、
    Android 2.3(Gingerbread)までのOSでは`res/layout/sample.xml`が適用され、
    Android 3.0(Honeycomb)以降のOSでは`res/layout-v11/sample.xml`が適用される。  
    また、あるバージョンで初めて導入されたXMLパラメータを使用したい場合も
    同様にファイルを分離する。

1.  minSdkLevelより高いレベルのAPIを使う時はチェックする

    minSdkLevelで設定したバージョンのOSにインストールされることを想定する。  
    新しいAPIを使う場合は、以下のようにバージョンをチェックして利用すること。

    ```java
    if (Build.VERSION.SDK_INT < Build.VERSION_CODES.JELLYBEAN) {
        // Android 4.0以下での従来のAPIでの処理
    } else {
        // Android 4.1で追加されたAPIでの処理
    }
    ```

1.  android-supportライブラリを使う

    比較的新しいバージョンで追加されたクラスには互換性のためのクラスが
    提供されていることがある。  
    そのAPIが導入される前の下位バージョンをサポートする必要がある場合は、  
    下手に自作しようとする前にandroid-supportライブラリで提供されていないか確認する。

    例:

    ```java
    android.support.v4.app.FragmentActivity
    android.support.v4.app.NotificationCompat.Builder
    ```

1.  SuppressWarningsやdeprecationアノテーションの適用範囲は最小限にする

    警告を抑えるためにアノテーションを付加することができるが、
    その場合は該当部分をメソッドに抽出してそのメソッドにのみ
    アノテーションを付けるなど、適用範囲を最小限にする。

    理由:

    * バージョン制御をし忘れてAPIを呼び出している箇所が他にあっても、
      警告の抑制によって見落とされてしまうため。  
      アノテーションの適用範囲を極小化して目視でのチェックが可能なように
      しておくことで、そのような不具合を避ける。
