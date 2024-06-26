---
title: Custom stores
---

オブジェクトが `subscribe` メソッドを正しく実装していれば、それは store です。それ以外は何でもありです。したがって、ドメイン固有のロジックを使ってカスタムの store を作成するのはとても簡単です。

たとえば、先ほどの例の `count` store に `increment`, `decrement`, `reset` メソッドを含めて、`set` と `update` は公開しないようにできます:

```js
/// file: stores.js
function createCount() {
	const { subscribe, set, update } = writable(0);

	return {
		subscribe,
		increment: () => +++update((n) => n + 1)+++,
		decrement: () => +++update((n) => n - 1)+++,
		reset: () => +++set(0)+++
	};
}
```
