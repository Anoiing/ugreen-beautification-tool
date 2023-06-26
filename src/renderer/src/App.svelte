<script lang="ts">
  import { Router, Route } from 'svelte-routing';
  import Statement from './Statement/index.svelte';

  export let url = '/';
</script>

<Router {url}>
  <div class="container px-8 py-6 mx-auto text-slate-100">
    <div class="flex justify-between mt-10 mb-16 select-none">
      <div>
        <div class="text-3xl font-bold">绿联NAS客户端美化工具</div>
        <div class="text-base text-slate-400">
          UGREEN NAS Client Beautification Tool
        </div>
      </div>
    </div>
    <Route path="/" component={Statement} />
  </div>
</Router>
