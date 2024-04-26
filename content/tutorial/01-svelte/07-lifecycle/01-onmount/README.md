---
title: onMount
---

すべてのコンポーネントには、作成される時を開始とし、破棄される時を終了とする _ライフサイクル_ があります。その重要なタイミングにコードを実行できるようにする関数がいくつかあります。最も頻繁に使用するのは `onMount` で、これはコンポーネントが最初に DOM にレンダリングされた後に実行されます。

この演習では、`gradient.js` の `paint` 関数 を使って `<canvas>` をアニメーションさせたいと思います。まずは `svelte` から `onMount` 関数をインポートしましょう:

```svelte
/// file: App.svelte
<script>
	+++import { onMount } from 'svelte';+++
	import { paint } from './gradient.js';
</script>
```

そして、コンポーネントがマウントされるときに実行されるコールバックを追加しましょう:

```svelte
/// file: App.svelte
<script>
	import { onMount } from 'svelte';
	import { paint } from './gradient.js';

+++	onMount(() => {
		const canvas = document.querySelector('canvas');
		const context = canvas.getContext('2d');+++

+++		requestAnimationFrame(function loop(t) {
			requestAnimationFrame(loop);
			paint(context, t);
		});
	});+++
</script>
```

> [後の演習](bind-this)で、`document.querySelector` を使わずに要素の参照を取得する方法を学びます。

ここまでは順調です — Svelte のロゴの形で、色がゆるやかに波打つのが見えるはずです。しかし1つ問題があります — コンポーネントが破棄されたとしても、その後 loop は動き続けてしまうのです。これを修正するため、`onMount` からクリーンアップ用の関数を返す必要があります:

```js
/// file: App.svelte
onMount(() => {
	const canvas = document.querySelector('canvas');
	const context = canvas.getContext('2d');

	+++let frame =+++ requestAnimationFrame(function loop(t) {
		+++frame =+++ requestAnimationFrame(loop);
		paint(context, t);
	});

+++	return () => {
		cancelAnimationFrame(frame);
	};+++
});
```
