---
title: Auto-subscriptions
---

前の例でアプリは動きましたが、微妙なバグがあります。store はサブスクライブされますが、決してアンサブスクライブされません。仮に、コンポーネントが何度もインスタンス化および破棄されるなら、_メモリリーク_ が発生することになるでしょう。

まず、`App.svelte` で `unsubscribe` を宣言します。

```js
/// file: App.svelte
+++const unsubscribe =+++ count.subscribe((value) => {
	count_value = value;
});
```

> `subscribe` メソッドを呼ぶと `unsubscribe` 関数が返ります.

`unsubscribe` が宣言されましたが、これをさらに、例えば `onDestroy` lifecycle hook で呼び出す必要があります。

```svelte
/// file: App.svelte
<script>
	+++import { onDestroy } from 'svelte';+++
	import { count } from './stores.js';
	import Incrementer from './Incrementer.svelte';
	import Decrementer from './Decrementer.svelte';
	import Resetter from './Resetter.svelte';

	let count_value;

	const unsubscribe = count.subscribe(value => {
		count_value = value;
	});

	+++onDestroy(unsubscribe);+++
</script>

<h1>The count is {count_value}</h1>
```

ただし、このやり方は、特にコンポーネントが複数の store にサブスクライブしている場合に、少し定型的になり始めます。代わりに、Svelte には巧妙な工夫が施されています。store の名前の前に `$` を付けることで、store の値を参照できます。

```svelte
/// file: App.svelte
<script>
	---import { onDestroy } from 'svelte';---
	import { count } from './stores.js';
	import Incrementer from './Incrementer.svelte';
	import Decrementer from './Decrementer.svelte';
	import Resetter from './Resetter.svelte';

	---let count_value;---

	---const unsubscribe = count.subscribe(value => {
		count_value = value;
	});---

	---onDestroy(unsubscribe);---
</script>

<h1>The count is {+++$count+++}</h1>
```

> 自動サブスクリプションは、コンポーネントの最上位スコープで宣言（またはインポート）された store 変数でのみ機能します。

`$count` の使用はマークアップ内に限定されません。イベントハンドラやリアクティブ宣言など、`<script>` のどこでも使用できます。

> `$` で始まる名前は store の値を参照していると見なされます。これは事実上の予約文字です。Svelte では `$` プレフィックスを使った独自変数を宣言できません。
