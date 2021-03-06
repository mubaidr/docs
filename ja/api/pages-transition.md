---
title: "API: pageTransition プロパティ"
description: Nuxt.js では transition コンポーネントを使って、ページ間を遷移する際のトランジション/アニメーションを行うことができます。
---

# pageTransition プロパティ

> Nuxt.js は [&lt;transition&gt;](http://vuejs.org/v2/guide/transitions.html#Transitioning-Single-Elements-Components) コンポーネントを使って、ページ間を遷移する際のトランジション/アニメーションを行うことができます。

> Nuxt v2.7.0 において layout の transition キーと名前を統合するため、"transition" キーよりも "pageTransition" キーのほうを使うようアナウンスされました。Nuxt v3.0.0 からは "transition" キーは非推奨となります。

- **型:** `String` または `Object` または `Function`

特定のルートに対してカスタムトランジションを設定するには、ページコンポーネントに `pageTransition` キーを追加してください。

```js
export default {
  // 文字列を指定できます
  pageTransition: ''
  // またはオブジェクト
  pageTransition: {}
  // または関数
  pageTransition (to, from) {}
}
```

## 文字列

`pageTransition` キーに文字列がセットされたときは `transition.name` として用いられます。

```js
export default {
  pageTransition: 'test'
}
```

上のように設定されると、コンポーネントは次のようにセットされます:

```html
<transition name="test">
```

## オブジェクト

`pageTransition` キーにオブジェクトがセットされたとき:

```js
export default {
  transition: {
    name: 'test',
    mode: 'out-in'
  }
}
```

上のように設定されると、コンポーネントは次のようにセットされます:

```html
<transition name="test" mode="out-in">
```

`transition` オブジェクトが持つことができるプロパティは以下のとおり:

| キー | 型 | デフォルト | 定義 |
|------|------|---------|-----------|
| `name` | `String` | `"page"` | すべてのトランジション時に適用されるトランジション名 |
| `mode` | `String` | `"out-in"` | すべてのトランジション時に適用されるトランジションモード。詳細は [Vue.js のドキュメント](http://vuejs.org/v2/guide/transitions.html#Transition-Modes) 参照 |
| `css` | `Boolean` | `true` | CSS トランジションクラスを適用するか否か。デフォルトは `true` です。false を設定すると、コンポーネントのイベントで登録された JavaScript フックのみがトリガーになります |
| `duration` | `Integer` | `n/a` | トランジションが適用される時間（ミリ秒）です。詳細は [Vue.js のドキュメント](https://vuejs.org/v2/guide/transitions.html#Explicit-Transition-Durations) 参照 |
| `type` | `String` | `n/a` | トランジション終了のタイミングを判定するために待ち受けるトランジションのイベントタイプを指定します。"transition" または "animation" を指定できます。デフォルトでは、より時間がかかるほうのタイプが自動的に選ばれます |
| `enterClass` | `String` | `n/a` | トランジション開始時の状態のクラスです。詳細は [Vue.js のドキュメント](https://vuejs.org/v2/guide/transitions.html#Custom-Transition-Classes) 参照 |
| `enterToClass` | `String` | `n/a` | トランジション終了時の状態のクラスです。詳細は [Vue.js のドキュメント](https://vuejs.org/v2/guide/transitions.html#Custom-Transition-Classes) 参照 |
| `enterActiveClass` | `String` | `n/a` | トランジション中に適用されるクラスです。詳細は [Vue.js のドキュメント](https://vuejs.org/v2/guide/transitions.html#Custom-Transition-Classes) 参照 |
| `leaveClass` | `String` | `n/a` | トランジション開始時の状態のクラスです。詳細は [Vue.js のドキュメント](https://vuejs.org/v2/guide/transitions.html#Custom-Transition-Classes) 参照 |
| `leaveToClass` | `String` | `n/a` | トランジション終了時の状態のクラスです。詳細は [Vue.js のドキュメント](https://vuejs.org/v2/guide/transitions.html#Custom-Transition-Classes) 参照 |
| `leaveActiveClass` | `String` | `n/a` | トランジション中に適用されるクラスです。詳細は [Vue.js のドキュメント](https://vuejs.org/v2/guide/transitions.html#Custom-Transition-Classes) 参照 |

`pageTransition` の中でメソッドを定義することもでき、メソッドは [JavaScript フック](https://vuejs.org/v2/guide/transitions.html#JavaScript-Hooks) で使われます:

- beforeEnter(el)
- enter(el, done)
- afterEnter(el)
- enterCancelled(el)
- beforeLeave(el)
- leave(el, done)
- afterLeave(el)
- leaveCancelled(el)

*メモ: JavaScript のみのトランジションのために明示的に `css: false` を追加しておくのは良いアイディアです。これは Vue は CSS 判定をスキップさせます。また誤って CSS ルールがトランジションに干渉するのを防ぎます。*

### トランジションモード

**ページのデフォルトの transition モードは Vue.js のデフォルトモードとは異なります。**. `pageTransition` モードはデフォルトで `out-in` がセットされます。leaving と entering のトランジションを同時に実行したいときは、`mode: ''` というように mode に空文字列を指定する必要があります。

```js
export default {
  pageTransition: {
    name: 'test',
    mode: ''
  }
}
```

## 関数

`pageTransition` キーに関数がセットされたとき:

```js
export default {
  pageTransition (to, from) {
    if (!from) return 'slide-left'
    return +to.query.page < +from.query.page ? 'slide-right' : 'slide-left'
  }
}
```

トランジションは各ページ遷移時に次のように適用されます:

- `/` から `/posts` へ遷移するとき => `slide-left`
- `/posts` から `/posts?page=3` へ遷移するとき => `slide-left`
- `/posts?page=3` から `/posts?page=2` へ遷移するとき => `slide-right`
