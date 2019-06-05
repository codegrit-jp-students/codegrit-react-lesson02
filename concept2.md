# Reactコンポーネントの書き方

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

- jsFiddleを利用した例

<iframe width="100%" height="300" src="//jsfiddle.net/codegrit_hiro/maLytch6/1/embedded/js,html,css,result/dark/" allowfullscreen="allowfullscreen" allowpaymentrequest frameborder="0"></iframe>

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

- jsFiddleを利用した例

<iframe width="100%" height="300" src="//jsfiddle.net/codegrit_hiro/L9x2d6nh/1/embedded/js,html,css,result/dark/" allowfullscreen="allowfullscreen" allowpaymentrequest frameborder="0"></iframe>

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