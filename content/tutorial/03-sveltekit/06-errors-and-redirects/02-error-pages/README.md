---
title: Error pages
---

`load` 関数の内側でなにか問題が発生したとき、SvelteKit はエラーページをレンダリングします。

デフォルトのエラーページは当たり障りないものです。`src/routes/+error.svelte` コンポーネントを作成することで、カスタマイズすることができます。

```svelte
/// file: src/routes/+error.svelte
<script>
	import { page } from '$app/stores';

	const emojis = {
		// TODO add the rest!
		420: '🫠',
		500: '💥'
	};
</script>

<h1>{$page.status} {$page.error.message}</h1>
<span style="font-size: 10em">
	{emojis[$page.status] ?? emojis[500]}
</span>
```

> `page` store を使用していますが、これは後の章で詳細に学習します。

`+error.svelte` コンポーネントは最上位(root)の `+layout.svelte` の内側でレンダリングされます。よりきめ細やかな `+error.svelte` 境界を作ることもできます。

```svelte
/// file: src/routes/expected/+error.svelte
<h1>this error was expected</h1>
```

このコンポーネントは `/expected` でエラーが発生した場合にレンダリングされ、その他の場所でエラーが発生した場合は最上位(root)の `src/routes/+error.svelte` ページがレンダリングされます。
