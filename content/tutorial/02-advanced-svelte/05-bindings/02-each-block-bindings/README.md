---
title: Each block bindings
---

`each` ブロック内のプロパティにバインドすることもできます。

```svelte
/// file: App.svelte
{#each todos as todo}
	<li class:done={todo.done}>
		<input
			type="checkbox"
			+++bind:+++checked={todo.done}
		/>

		<input
			type="text"
			placeholder="What needs to be done?"
			+++bind:+++value={todo.text}
		/>
	</li>
{/each}
```

> これらの `<input>` 要素と互いに作用すると、配列が突然変化することに注意してください。不変のデータを扱いたい場合は、バインディングを避け、代わりにイベントハンドラを使用する必要があります。
