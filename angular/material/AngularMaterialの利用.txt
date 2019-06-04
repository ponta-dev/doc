HP
https://material.angular.io/

○　インストール
npmでプロジェクトにローカルインストール
npm install --save @angular/material @angular/cdk @angular/animations
+ @angular/animations@7.0.1
+ @angular/material@7.0.2
+ @angular/cdk@7.0.2

ng add @angular/materialでもいけるらしい（AngularCLI6以上）

○　アプリへの組み込み
app.module.tsの方にアニメーションモジュールを組み込む
import {BrowserAnimationsModule} from '@angular/platform-browser/animations';
@NgModule({
  ...
  imports: [BrowserAnimationsModule],
  ...
})

アニメーションが嫌な場合はNoopAnimationsModuleを組み込む

次にマテリアルのコンポーネントを同様にインポート
import {MatButtonModule, MatCheckboxModule} from '@angular/material';
@NgModule({
  ...
  imports: [MatButtonModule, MatCheckboxModule],
  ...
})

ルーティングモジュールみたいにモジュールだけ切り離してルートモジュールに組み込んでもよい（そうすべき？）
ng generate module app-material --flat --module=app
でマテリアル用のモジュールを組み込む
マテリアルモジュールはBrowserModuleの後に必ず組み込む（順序大事らしい）
上のBrowserAnimationsModuleはルートモジュールに組み込む必要がある
MatButtonModuleとかはフィーチャー（app-material）モジュールの方に切り分けた方が良い
MatButtonModuleとかは使うときに組み込めばよくて初めから入れる必要はない

○　CSSテーマの組み込み
AngularCLIを利用している場合、
@import "~@angular/material/prebuilt-themes/indigo-pink.css";
をstyles.cssに突っ込む

○　その他ライブラリの組み込み
mat-slide-toggle, mat-slider, matTooltip
この辺のモジュールを使う場合（使わなくても？）hammerjsに依存しているのでアプリに組み込む必要がある
npm install --save hammerjs
いれた場合はsrc/main.tsにimport 'hammerjs';する

○　アイコンの組み込み
マテリアルのアイコン（ブートストラップとかにもあるやつ）を使う場合は
index.htmlにアイコンへのリンクを張る
<link href="https://fonts.googleapis.com/icon?family=Material+Icons" rel="stylesheet">