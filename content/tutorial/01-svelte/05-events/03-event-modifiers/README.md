---
title: Event modifiers
---

DOM イベントハンドラには、それらの動作を変更する修飾子（*modifiers*）を設定することができます。たとえば、`once` 修飾子をハンドラに設定すると、1回だけ実行します。

```svelte
/// file: App.svelte
<button on:click+++|once+++={() => alert('clicked')}>
	Click me
</button>
```

イベント修飾子の一覧:

* `preventDefault` — ハンドラを実行する前に `event.preventDefault()` を呼び出します。たとえば、クライアントのフォーム処理に役立ちます。
* `stopPropagation` — 次の要素にイベントが伝播しないように `event.stopPropagation()` を呼び出します。
* `passive` — タッチ/ホイールイベントでスクロールのパフォーマンスを向上させます（Svelte が安全な場所に自動的に追加します）。
* `nonpassive` — `passive: false` を明示的に設定します。
* `capture` — [_バブリング(bubbling)_](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Building_blocks/Events#event_bubbling) フェーズではなく、[_キャプチャ(capture)_](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Building_blocks/Events#event_capture) フェーズ中にハンドラを起動します。
* `once` — ハンドラを最初に実行した後に削除します。
* `self` — 設定した要素が event.target の場合にのみ、ハンドラをトリガします。
* `trusted` — `event.isTrusted` が `true` の場合にのみハンドラをトリガします。つまり、JavaScript が `element.dispatchEvent(...)` を呼び出した場合は `true` にならず、ユーザーのアクションによってイベントがトリガされた場合は `true` になります。

イベント修飾子を連結することができます。（例）`on:click|once|capture={...}`
