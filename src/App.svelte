<script lang="ts">
  import svelteLogo from './assets/svelte.svg'
  import viteLogo from '/vite.svg'
  import Counter from './lib/Counter.svelte'
  import type { DocType } from './lib/doc-type';

  import { Counter as AutomergeCounter } from '@automerge/automerge';
  import { DocHandle, Repo, isValidAutomergeUrl } from '@automerge/automerge-repo';
  import { BroadcastChannelNetworkAdapter } from '@automerge/automerge-repo-network-broadcastchannel';
  import { IndexedDBStorageAdapter } from '@automerge/automerge-repo-storage-indexeddb';
  import { BrowserWebSocketClientAdapter} from '@automerge/automerge-repo-network-websocket'
  import { setContextRepo } from '@automerge/automerge-repo-svelte-store';
  import Message from './lib/Message.svelte';

  const repo = new Repo({
    network: [
      new BroadcastChannelNetworkAdapter(),
      new BrowserWebSocketClientAdapter(`ws://localhost:4000`)
    ],
    storage: new IndexedDBStorageAdapter(),
  })

  setContextRepo(repo)
  
  const rootDocUrl = `${document.location.hash.substring(1)}`
  let handle: DocHandle<DocType>;
  if (isValidAutomergeUrl(rootDocUrl)) {
    handle = repo.find(rootDocUrl)
  } else {
    handle = repo.create<DocType>({ message: '', count: new AutomergeCounter() })
  }

  const docUrl = (document.location.hash = handle.url)

</script>

<main>
  <div>
    <a href="https://vitejs.dev" target="_blank" rel="noreferrer">
      <img src={viteLogo} class="logo" alt="Vite Logo" />
    </a>
    <a href="https://svelte.dev" target="_blank" rel="noreferrer">
      <img src={svelteLogo} class="logo svelte" alt="Svelte Logo" />
    </a>
  </div>
  <h1>Vite + Svelte</h1>

  <div class="card">
    <Counter {docUrl} />
    <Message {docUrl} />
  </div>

  <p>
    Check out <a href="https://github.com/sveltejs/kit#readme" target="_blank" rel="noreferrer">SvelteKit</a>, the official Svelte app framework powered by Vite!
  </p>

  <p class="read-the-docs">
    Click on the Vite and Svelte logos to learn more
  </p>
</main>

<style>
  .logo {
    height: 6em;
    padding: 1.5em;
    will-change: filter;
    transition: filter 300ms;
  }
  .logo:hover {
    filter: drop-shadow(0 0 2em #646cffaa);
  }
  .logo.svelte:hover {
    filter: drop-shadow(0 0 2em #ff3e00aa);
  }
  .read-the-docs {
    color: #888;
  }
</style>
