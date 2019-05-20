# レッスン2. JSXとReactコンポーネント

## サンプルコード

このレッスンで利用するサンプルコードはこちらのリポジトリよりご覧頂けます。より理解を深めるためコードをクローンして確認しましょう。

[codegrit-react-lesson02-samples](https://github.com/codegrit-jp-students/codegrit-react-lesson02-samples)

## React要素のネスト

さて、レッスン1ではcreateElementというファンクションを利用してReact要素の作成を行いました。しかし、実際のサイトでは、h1のみを表示することはなく、多くの要素を組み合わせることになります。例えば以下のようなHTMlを考えてみましょう。

```html
<main>
  <div class="test">
    <p>これはテストの説明です。</p>
  </div>
</main>
```

これをcreateElementを利用して表現しようとすると次のようになります。

```js
// src/WithCreateElement.js

import { createElement, Component } from 'react';

export default class extends Component  {
  render() {
    return (
      createElement('main', null,
        createElement('div', { class: "test" },
          createElement('p', null, "これはテストの説明です。")
        )
      )
    );
  }
}
```

これを見て、直感的に分かりやすいと感じたでしょうか？ 多くの方は分かりづらいと感じたかと思います。

Reactでは、こうしたReactエレメントの作成をcreateElementではなく**JSX**という記法を利用して置き換えることができます。

## JSXとは

JSXは、createElementを簡単に書くために作られた構文(シンタックスシュガー)です。JSXで書かれたコードはBabelによって通常のJavaScriptコードへとトランスパイルされます。create-react-appを利用しているとトランスパイルは自動で行ってくれますので意識しなくて大丈夫です。

JSXを利用することで以下のようなメリットを得ることができます。

1. **可読性が上がる**
JSXは見慣れたHTMLに近い見た目をしておりまた後述の通りコード量が少なくなるため、可読性が上がります。

2. **コード量が減る**
createElement関数を毎回呼び出すのに比べてコード量を減らすことができます。

3. **チーム開発の効率が上がる**
デザイナーでもそこに何が書かれているかの確認や簡単な修正が可能なため技術レベルのバラバラなチームでも効率よく開発ができます。

## JSXを試してみよう

では実際に最初のコードを書き換えてみましょう。すると以下のようになります。

```js
// src/WithJsx.js

import React from 'react' // JSXを含むファイルには必ずReactを呼ぶ必要があります。

const jsxEx = (
  <main>
    <div className="subscription">
      <p>これはテストの説明です。</p>
    </div>
  </main>
);
```

ご覧の通り、ほとんど通常のHTMLに近い見た目をしているかと思います。しかし、よく見ると通常`class`となっている部分が`className`になっていることに気づくかと思います。これはclassがJavaScriptの予約後になっているためです。この他にもJSX独自の書き方があり、これらの書き方になれていく必要があります。次の節では、こうした書き方の一部について簡単に見ていきましょう。書き方については最初からすべてを覚える必要はなく、Reactの学習を進めて行く中で少しずつ調べたりして身につけていくのが良いでしょう。

## JSXの書き方

### コンポーネント内にJSXを書く

コンポーネント内で通常JSXはrenderファンクション内に書きます。この際複数行に渡ってJSXを書く場合はカッコ()でJSXの部分を囲います。

```js
// src/WithJsx.js

import React, { Component } from 'react'

export default class extends Component {
  render() {
    return (
      <main>
        <div className="test">
          <p>これはテストの説明です。</p>
        </div>
      </main>
    );
  }
}
```

### 変数を利用する

JSX内で変数を利用したい場合には変数をカギカッコ{}で囲みます。

```js
// src/Weather.js

import React, { Component } from 'react'

export default class extends Component {
  render() {
    const weather = "晴れ"
    return <p>今日の天気は{weather}です。</p>
  }
}
```

### 既存のコンポーネントをJSX内で呼び出す。

既存のコンポーネントをJSX内に含める際は以下のように書きます。

```js
// src/UseWeather.js

import React, { Component } from 'react'
import Weather from './Weather' // Weatherコンポーネントをインポートする。

export default class extends Component {
  render() {
    return (
      <div>
        <h1>天気のコンポーネントを呼び出します。</h1>
        <Weather />
      </div>
    );
  }
}
```


### 既存のコンポーネントにPropsを渡す

JSX内で呼び出すコンポーネントにPropsを渡したい場合、２つの書き方が可能です。

1. プロパティ名と値を直接渡す。
2. spread構文を利用して値を渡す。

**1の書き方の例:**

```js
// src/PassPropsDirectly.js

import React, { Component } from 'react';
import HelloWorld from './HelloWorld';

export default class extends Component {
  render() {
    return (
      <main>
        <h1>"こんにちは 世界"と表示します。</h1>
        <HelloWorld hello='こんにちは' world='世界' />
      </main>
    )
  }
}
```

**2の書き方の例:**

```js
// src/PassPropsWithSpread.js

import React, { Component } from 'react'
import HelloWorld from './HelloWorld';

export default class extends Component {
  render() {
    const props = {
      hello: 'こんにちは',
      world: '世界',
    }
    return (
      <main>
        <h1>"こんにちは 世界"と表示します。</h1>
        <HelloWorld {...props} />
      </main>
    );
  }
}
```

### コメントを書く。

JSX内でコメントを書くにはコメントにしたい部分を`{/* */}`で囲みます。

```js
// src/HelloWorld.js

import React from 'react';

const HelloWorld = ({ hello, world }) => (
  <div>
    {/* この部分はコメントです。 */}
    <h2>{hello} {world}</h2>
  </div>
)

export default HelloWorld;
```

### classを利用する

classを利用したい場合は、既に見てきた通り`className`を利用します。

```js
// src/WithJsx.js

import React, { Component } from 'react'

export default class extends Component {
  render() {
    return (
      <main>
        <div className="test">
          <p className="explanation">これはテストの説明です。</p>
        </div>
      </main>
    );
  }
}
```

### インラインスタイルを利用する

インラインスタイルはオブジェクトリテラルを利用して書きます。書き方は少し特別で、プロパティ名でハイフン(-)が入る場合はその部分のハイフンをなくして次の文字を大文字にします。また値は""で囲んであげる必要があります。(数字の場合を除く)

```js
// src/WithInlineStyle.js

import React, { Component } from 'react'

export default class extends Component {
  render() {
    // styleをオブジェクトを利用して定義。
    const inlineStyle = {
      fontSize: '14px',
      color: 'red',
      textAlign: 'left',
      fontWeight: 600
    }
    return <p style={inlineStyle}>インラインスタイル適用</p>
  }
}
```

### 閉じるための要素の省略

例えば、以下のようなJSXを考えてみましょう。

```js
import testImg from './images/test.jpg'; // イメージをインポートする。

<img src={testImg}></img>
```

この時、`<img></img>`の要素間(一般的にchildrenあるいは子要素と呼びます。)には何も入っていません。こうした場合は以下のように書くことができます。

```js
<img src={testImg} />
```

このように閉じるための要素を省略する場合には`/>`と最後に書きます。また`/`の前には必ずスペースを一個入れて可読性を上げるようにしましょう。

### Booleanを利用する

例えばフォームで入力をできないようにする際に`disabled`という値を渡します。この際JSXでは以下のような書き方ができます。

```js
<input type="email" disabled={true} />
```

また、JSXではPropsのデフォルト値は`true`となっています。そのため上記のコードは以下のように省略できます。

```js
<input type="email" disabled />
```

### ブラウザイベント(click, hoverなど)に対応する

イベントに対応するには、イベント名の前にonをつけ、イベント名の最初の文字を大文字にします(例: onClick, onHover)。 

```js
// イベント処理を同時に定義する
<a onHover={(e) => console.log("hovered")} />
<a onClick={(e) => console.log("clicked")} />

// コンポーネントのクラスファンクションを呼び出す
<a onClick={this.handleClick} />

// Propsのファンクションを呼び出す
<a onClick={this.props.handleClick} />

// 引数をつけて渡す
<a onClick={(e) => this.handleClick(e, 'red') />
<a onClick={(e) => this.handleClick(e, { type: 'modal' }) />
<a onClick={(e) => this.props.handleClick(e, { type: 'modal' })} />
```

## Reactコンポーネントの2つの書き方

さて、ここまでの例ではReactコンポーネントを書く際にclassを利用した書き方を紹介してきました。ここでは、もう一つのファンクションを利用した書き方と合わせて再度紹介していきます。

### 1. classを利用した書き方

一個目はこれまでにも利用してきたclassを利用した書き方です。

```js
// src/ComponentWithClass.js

import React, { Component } from 'react'

class Index extends Component {

  getSentence = () => {
   return <span>Hello, {this.props.word}</span>
  }

  render () {
    return <p>{this.getSentence()}</p>
  }
}

export default Index
```

上記のように、渡されたpropsへとアクセスするためには`this.props`という呼び出し方をしている点に注意してください。またファンクションを利用してJSX内の値をダイナミックに変更することも可能です。

### 2. ファンクションを利用した書き方

ファンクションを利用する場合、以下のように書くことができます。

```js
// src/ComponentWithFunction.js

import React from 'react'

const Index = (props) => {
  return <p>Hello, {props.word}</p>
}

export default Index
```

classを利用した場合と異なり、renderファンクションの呼び出しが省略できるので、よりスッキリとした見た目にすることが可能です。また引数にpropsを直接設定できる点にも注目してください。propsの数が少ない場合には分割代入を利用して以下のように書くこともできます。

```js
import React, { Component } from 'react'

const Index = ({ word }) => {
  return <p>Hello, {word}</p>
}

export default Index
```

## コンポーネントのインポートに関する注意点

コンポーネントをインポートする上で重要な点は、コンポーネント名は必ず大文字から始まることです。そうしないとReactがコンポーネントをコンポーネントと認識してくれませんので注意してください。またコンポーネントをインポートする際にも最初の文字には大文字を利用しましょう。

- 間違った例

```js
import React from 'react';
import weather from './Weather'; // コンポーネント名が小文字になっている

const Index = () => {
  return (
    <div>
      <h1>天気のコンポーネントを呼び出します。</h1>
      <weather /> {/* 何も表示されません。 */}
    </div>
  );
}

export default Index;
```

- 正しい例

```js
import React from 'react'
import Weather from './Weather'

const Index = () => {
  return (
    <div>
      <h1>天気のコンポーネントを呼び出します。</h1>
      <Weather />
    </div>
  );
}

export default Index;
```

## 更に学ぼう

- [React公式 - JSXの導入](https://ja.reactjs.org/docs/introducing-jsx.html)