<script lang="ts">
  import {document} from "@automerge/automerge-repo-svelte-store"
  import type { AutomergeUrl } from "@automerge/automerge-repo";
  import type { DocType } from "./doc-type";

  export let docUrl:AutomergeUrl;
  const doc = document<DocType>(docUrl)

  const updateValue = (e: KeyboardEvent) => {
    const newValue = (e.target as HTMLInputElement).value
    doc.change(doc => {
      doc.message = newValue
    })
  }
</script>

<div>
  <label for="message">Type a message</label>
  <input type="text" name="message" id="message" value={$doc?.message} on:keyup={updateValue}>
</div>

<style>
  div {
    display: inline-flex;
    flex-flow: column;
    gap: 0.5em;
  }

  label {
    align-self: flex-start;
  }
  
  input {
    font-family: inherit;
    font-size: 1rem;
    padding: 0.5em;
    border-radius: 0.5em;
    border: hsl(191, 21%, 66%) 0.1em solid;
  }
</style>