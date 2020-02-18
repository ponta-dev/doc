# v-if
  DOMの生成の切り替えを行う
  templateに対してv-ifを付与するとtemplate内の複数の要素に対してv-ifが適用される
```html
<span v-if="seen">Now you see me</span>

<template v-if="ok">
  <h1>Title</h1>
  <p>Paragraph 1</p>
  <p>Paragraph 2</p>
</template>
```
  v-elseでelseの場合の指定ができる
  v-elseは前にv-ifかv-else-ifがない場合は認識されない
```html
<div v-if="Math.random() > 0.5">
  Now you see me
</div>
<div v-else>
  Now you don't
</div>
```

## 条件付きレンダリング
  Vueではv-ifの制御の中で、同一のコンポーネント内の要素の使いまわしを行うことがある
  例えばinputの入力内容が引き継がれたりする
  この挙動が望ましくない場合、明示的に使いまわさない設定が必要
  key属性でIDを与えると切り分けを行う
  labelはkey属性を持たないので基本使いまわされる
```html
<template v-if="loginType === 'username'">
  <label>Username</label>
  <input placeholder="Enter your username" key="username-input">
</template>
<template v-else>
  <label>Email</label>
  <input placeholder="Enter your email address" key="email-input">
</template>
```
## v-show
  v-ifがDOMごとの制御を行うことに対し、v-showは表示非表示をCSS的に行う
```html
<h1 v-show="ok">Hello!</h1>
```
  v-ifとv-showの使い分けは、コストを初期描画時にしたい場合はv-show
  それ以外のケースではv-ifを使う
  v-showはv-elseとの連携やtemlplateによる複数制御は出来ない

# v-for
  値分だけ表示する
  item in itemsの形式で特別な構文を要求する
  itemsはデータの配列
  v-ifと同時に利用することは推奨されない（同じDOMにv-ifとv-forを指定することが推奨されない）
  フィルタリングなどは算出プロパティを定義し、そちらで行う
```javascript
<ul id="example-1">
  <li v-for="item in items">
    {{ item.message }}
  </li>
</ul>
var example1 = new Vue({
  el: '#example-1',
  data: {
    items: [
      { message: 'Foo' },
      { message: 'Bar' }
    ]
  }
})
```
  v-forブロック内では、親スコープのプロパティへの完全なアクセス権を持つ
  また配列のインデックスを2番目の引数として持っている
```html
<ul id="example-2">
  <li v-for="(item, index) in items">
    {{ parentMessage }} - {{ index }} - {{ item.message }}
  </li>
</ul>
```

## オブジェクトのv-for
  オブジェクトに対してもv-forが出来る
  その場合オブジェクトの各プロパティがアイテムとして渡される
  オブジェクトの場合2つ目の引数はプロパティ名
  3つ目の引数がインデックスになる
  Object.keys()による実装のため、順番はブラウザによって異なる可能性がある
```html
<div v-for="(value, name, index) in object">
  {{ index }}. {{ name }}: {{ value }}
</div>
```

## 更新の検知
  v-forが参照する配列の値がかわった場合、DOM要素の更新が行われる
  その場合リスト表示されているコンポーネントの状態に依存しているケースでは問題がある
  Vueが各コンポーネントの追跡ができるように、key属性を指定するのが望ましい
  keyの値はオブジェクトなどの非プリミティブなデータを使わないこと
```html
<div v-for="item in items" v-bind:key="item.id">
  <!-- content -->
</div>
```

  配列の更新を検知するための変更メソッドが用意されている（標準をラップしている）
  * push()
  * pop()
  * shift()
  * unshift()
  * splice()
  * sort()
  * reverse()

  検知に関して、下記のケースではVueは検知できない
  * インデックスで指定したアイテムを直接書き換える
  * 配列の長さを変更する
  このケースの解法として、Vue.set（またはvm.$set）を使ってリストを更新するか、
  splice()のラッパー関数を使う
  オブジェクトの場合も同様にプロパティの追加や削除を検知できない
  Vueでは既に作成されたインスタンスに、新しいルートレベルのリアクティブプロパティを動的に追加はできない
  ただVue.set(object, propertyName, value)でネストされたオブジェクトにリアクティブなプロパティを追加はできる
  
  
  配列自体を新しいものに置き換えるケースとしてfilterやconcatのように元の配列は変更しないが、
  新しい配列を返すメソッドを使うケースがある
  その場合、古い配列を新しい配列で置き換えるべき
  配列全体を置き換えた場合の挙動として、VueではDOM全体を再描画するようなことはしない
  配列全体の置き換えは推奨されている
  元の配列は変更せずにフィルタリングのみしたい場合は算出プロパティによるフィルタリングと、
  そこから返される配列の参照を行う
```javascript
<li v-for="n in evenNumbers">{{ n }}</li>
data: {
  numbers: [ 1, 2, 3, 4, 5 ]
},
computed: {
  evenNumbers: function () {
    return this.numbers.filter(function (number) {
      return number % 2 === 0
    })
  }
}
```

## 範囲付きv-for
  配列ではなく整数値をとることができる
```html
<div>
  <span v-for="n in 10">{{ n }} </span>
</div>
```

## templateのv-for
  templateにv-forを指定し、複数の要素をブロックをレンダリングできる
```html
<ul>
  <template v-for="item in items">
    <li>{{ item.msg }}</li>
    <li class="divider" role="presentation"></li>
  </template>
</ul>
```

## componentに対するv-for
  componentに対してもv-forは利用できる
  componentに対してv-forを行う場合、key属性は必須になる
  またcomponentに対して渡すデータは明示的に行う必要がある
  これはcomponentが独自のデータ範囲を持っているからで、コンポネント性を高めるために必要
```html
<div id="todo-list-example">
  <form v-on:submit.prevent="addNewTodo">
    <label for="new-todo">Add a todo</label>
    <input
      v-model="newTodoText"
      id="new-todo"
      placeholder="E.g. Feed the cat"
    >
    <button>Add</button>
  </form>
  <ul>
    <li
      is="todo-item"
      v-for="(todo, index) in todos"
      v-bind:key="todo.id"
      v-bind:title="todo.title"
      v-on:remove="todos.splice(index, 1)"
    ></li>
  </ul>
</div>
```
