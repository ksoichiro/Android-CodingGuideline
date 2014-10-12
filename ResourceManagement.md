# ライフサイクル/リソース管理

1.  Fragmentにフィールドを持たせない

    `Fragment`のフィールドは、ライフサイクルにより
    オブジェクトが再生成されるタイミングで破棄される。  
    `Fragment`にパラメータを付与したい場合はフィールドでなく
    `setArguments()`によって`Bundle`オブジェクトとして渡す必要があり、
    かつ`Fragment`がアクティブになる前に設定する必要がある。
    フィールドに直接設定すると、`FragmentManager`等に再生成された時に
    内容が失われる。  
    `Bundle`オブジェクトに登録できない類のオブジェクトが必要な場合、
    それらを必要な時に生成するリスナーを定義する。

    悪い例:

    ```java
    class SampleFragment extends Fragment {
        View mView; // このviewは再生成時にnullになり復元されない
        void setView(View view) {
            mView = view;
        }
        public View onCreateView(...) {
            :
            mView.findViewById(...); // mViewはnullの可能性がある
            :
        }
    }
    ```

    良い例:

    ```java
    class SampleFragment extends Fragment {
        public static interface ViewProvider {
            View onCreateView();
        }
        public View onCreateView(...) {
            :
            // このFragmentを所有するActivityにViewを生成させる
            if (getActivity() != null && getActivity() instanceof ViewProvider) {
                View v = ((ViewProvider) getActivity()).onCreaetView();
                v.findViewById(...);
            }
            // このFragmentを所有するFragmentにViewを生成させる
            Fragment f = getTargetFragment();
            if (f != null && f instanceof ViewProvider) {
                View v = ((ViewProvider) f).onCreaetView();
                v.findViewById(...);
            }
            :
        }
    }
    ```

1.  FragmentとActivityに依存関係を持たせない

    `Fragment#getActivity()`はnullの場合がある。  
    また逆に、`FragmentViewPager`を使って`Fragment`を複数管理し、
    `Activity`から特定の`Fragment`を操作しようとする場合などで、
    参照が無効になっている場合がある。
    できる限り、`Activity`と`Fragment`は互いに依存しないようにする。

1.  ArrayAdapter#getView()で生成するビューは再利用される

    `ListView`の行のビューを生成する`ArrayAdapter#getView()`では、
    他の行のデータが設定済みのビューが使われなくなった時点で
    別の行へ使い回される。
    したがって、`getView()`の時点でのデータを使用してイベントリスナーの
    処理等を記述すると別の行のデータを扱ってしまう可能性があることに注意する。

    例:

    ```java
    public View getView(…) {
        View view = super.getView(…);
        final Person p = getItemAt(position);
        ((TextView) view.findViewById(R.id.name))
                .setText(p.getName());
        view.findViewById(R.id.btn).setOnClickListener(
            new View.OnClickListener {
                @Override
                public void onClick(View v) {
                    // この時点でこのボタンは
                    // 別のデータを割り当てられた行の
                    // ボタンになっている可能性あり。
                    // p.getName()は表示している内容と
                    // 一致しない可能性がある
                }
            });
    }
    ```

1.  Dialog#show()を直接呼び出さない

    Android 3.0以降(もしくはandroid-support-v4.jar利用)の場合は`DialogFragment`を使う。
    それ以外の場合は、`Activity`のライフサイクルメソッドから呼び出すこと。

1.  Activity#onPause() または Activity#onStop() で破棄する

    Activity内で確保したリソースの解放はできれば`onPause()`メソッド、
    遅くとも`onStop()`メソッドで行うこと。  

    理由:

    * `onPause()`や`onStop()`が呼び出されたタイミングで、
      確保したリソース(特にハードウェア)を
      別のActivityやアプリが使用しようとしている可能性があるため。

1.  Application#onDestroy()を終了処理に使わない

    `Application#onDestroy()`はAPIとして存在しているものの実機では動作しない。
    したがって、このメソッドをアプリケーション全体の終了処理に利用するのではなく、
    より早い段階で終了処理を行なうことを検討する。
    (`Activity#onStop()`など)

1.  Bitmapはrecycle()してnullにする

    `Bitmap`を生成した後は、利用が終わった後に`recycle()`メソッドにより解放し、
    さらにフィールドとして保持している場合は`null`を代入して
    ガベージコレクションされるようにすること。  

    理由:

    * 大抵の場合、`Bitmap`は他のオブジェクトに比べてメモリ使用量が大きく、
      積み重なると`OutOfMemoryError`等の致命的な問題に繋がるため、
      不要になり次第すぐに解放する必要がある。

1.  引数ContextにActivityを渡さない

    引数に`Context`を取るメソッドに対して、原則として`Activity`を`Context`として渡さないこと。  
    ただし、そのメソッドを持つオブジェクトがユーティリティ系メソッドで
    すぐに`Activity`の参照を解放する場合や、
    そのメソッドを持つオブジェクトが`Activity`のライフサイクルと
    同期している場合は問題ない。  
    大抵のケースでは、`Activity#getApplicationContext()`を渡せば良い。

    理由:

    * 引数に渡した`Activity`がライフサイクルを終えても、
      そのオブジェクトに参照を保持されているためにガベージコレクションされず、
      メモリリークを引き起こすため。

    注意:

    * `Context`を渡すオブジェクトがUI部品である場合、
      `Activity`を渡さないと、適切なStyleが適用されない場合がある。

1.  内部クラスはstaticにする

    内部クラスを定義する場合、親になるクラスとライフサイクルが
    完全に同期するのでない限りstaticクラスにする。
    特に、`Activity`や`Fragment`はライフサイクルによってインスタンスが再生成
    され、参照が無効になることがあるため非staticな内部クラスから
    親の`Activity`や`Fragment`を参照するとバグにつながる。

    理由:

    * 非staticな内部クラスを作ると、そのインスタンスは親クラスの参照を
      保持し続けるためメモリリークに繋がる。

1.  UI設定はタイミングに気をつける

    `Activity#requestWindowFeature()`やテーマの変更など、一部のUI変更メソッドは
    `Activity#setContentView()`より前に呼び出さなければ反映されないことに注意する。

1.  匿名クラス、内部クラスの内部で参照する親クラスの変数のライフサイクルを考慮する

    匿名クラスや内部クラスでは、その呼出し元メソッドの変数がfinalであるか、
    フィールドであればアクセスすることができるが、実際にそのメソッドが
    動作するタイミングで有効な参照であるかどうかを考慮すること。

    悪い例:

    ```java
    public class SampleFragment() {
        private Param mParam;
        public void onCreate() {
            :
            mParam = new Param();
            findViewById(R.id.btn).setOnClickListener(
                new View.OnClickListener() {
                    mParam.doSomehting();
                    // この匿名クラスが暗黙的に参照している
                    // SampleFragment.thisは
                    // 再生成されてmParamがnull
                    // になっている可能性がある

                    getActivity().finish();
                    // R.id.btnをタップしたタイミングでは
                    // getActivity()はnullの可能性がある
                });
        }
    }
    ```

