<script lang="ts">
  import {document} from '@automerge/automerge-repo-svelte-store'
  import type { AutomergeUrl } from "@automerge/automerge-repo";
  import type { DocType } from "./doc-type";
  export let docUrl: AutomergeUrl;

  const doc = document<DocType>(docUrl)
  const increment = () => {
    doc.change(d => {
      d.count.increment(1)
    })
  }
</script>

<button on:click={increment}>
  count is {$doc?.count || 0}
</button>
