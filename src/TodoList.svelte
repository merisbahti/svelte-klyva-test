<script lang="ts">
  import { get, writable } from 'svelte/store'
  import type { Writable } from 'svelte/store'
  import { slicedStore } from './klyva-svelte'
  import TodoItem from './TodoItem.svelte'
  export let todoItems: Writable<
    Array<{
      checked: boolean
      text: string
    }>
  >
  const currentTodoText = writable('')

  const createTodo = () => {
    todoItems.update((v) => [
      ...v,
      { checked: false, text: get(currentTodoText) },
    ])
    currentTodoText.set('')
  }

  const keyDown: svelte.JSX.EventHandler<KeyboardEvent, HTMLInputElement> = (
    e,
  ) => {
    if (e.key === 'Enter') createTodo()
  }
  let sliced = slicedStore(todoItems)
</script>

<ul>
  <input bind:value={$currentTodoText} on:keydown={keyDown} />
  <button on:click={createTodo}>Add new</button>
  {#each $sliced as slice}
    <TodoItem todoStore={slice} remove={slice.remove} />
  {/each}
</ul>
