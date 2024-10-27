# はじめに
RustでRaspberry Pi Picoをno_stdで走らせるためのサンプルコードです。オリジナルの方にREADMEが無いので勝手に書きました、導入の参考にどうぞ。

# 前提条件
1. Wokwi Simulatorプラグインをインストールします  
![](meta/wokwi_vs_simlator.png)
2. 以下のコマンドで必須コマンドをインストールします(cmakeが必要なので、必要時応じて[インストールします](https://qiita.com/matskeng/items/c466c4751e1352f97ce6))
    ```
    cargo install probe-rs-tools --locked
    cargo install flip-link elf2uf2-rs
    ```
3. `Ctrl+Shift+B` でビルドコマンドを走らせます。
    または以下のコマンドを実行します。（面倒なのでCtrl+Shift+Bのほうがおすすめです）このあたりの設定は`.vscode/tasks.json`に記述されています。
    ```
    cargo build
    elf2uf2-rs target/thumbv6m-none-eabi/debug/pico-wokwi-playground
    ```
4. `F1`もしくは`Ctrl+Shift+P`で`>Wokwi: Start Simulator`選択し、Wokwiシミュレータを起動します。（初めての人はここらへんでライセンス認証が走るので、GitHubとかのアカウントでSSOログインすると認証できます。）  
![](./meta/wokwi.png)

無事LEDがチカチカしてればOKです。

# 補足情報
## 導入するツールの説明
`probe-rs-tools` : デバッグツールです。物理マシンにファームウェアを書き込んだりデバッグするための機能を持ちます。  
`flip-link` : ベアメタル環境においてスタックバッファオーバーフローを検知するためのリンカーです。原理としてはメモリレイアウトをスタックのセクションと.bss及び.dataセクション入れ替えることで、オーバーフローが発生した場合にRAMの境界に書き込みが発生するように再構成します。RAMの境界に書き込みが発生するとハードフォルトが発生するので、そこにフォルトハンドラを定義しておくことで検知が可能になるという理屈になっているようです。  
`elf2uf2-rs` : Raspberry pi picoの場合、最終生成物はuf2というファームウェアになります。Rustが生成するのはあくまでもelfであるので変換しなければなりません。このツールはビルドが完了した後にelfをuf2（ファームウェア形式）に変換する部分を担います。

# 配線の変更
`diagram.json`を変更してください。テンプレートはWokwiにあるので参考にすると良いと思います。
