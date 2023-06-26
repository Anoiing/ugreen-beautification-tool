<script lang="ts">
  import { navigate } from 'svelte-routing';
  import { onMount } from 'svelte';
  import Button from '../components/Button/index.svelte';
  import img from '../assets/undraw_re_8gea.svg';

  let count = 10;
  let disabled = true;

  $: {
    if (count <= 0) {
      disabled = false;
    }
  }

  onMount(() => {
    // 10s阅读使用须知倒计时
    let t = window.setInterval(() => {
      if (count > 0) {
        count -= 1;
      } else {
        window.clearInterval(t);
      }
    }, 1000);
  });

  // 同意并继续
  const onCountinue = () => {
    navigate('/unpacking-factory');
  };
  // 不同意并退出
  const onDisagree = () => {
    window.electron.ipcRenderer.send('exit');
  };
</script>

<div
  class="flex flex-col items-center px-10 py-8 shadow-xl select-none bg-slate-900 rounded-xl"
>
  <div class="relative w-32 h-20">
    <img class="w-full h-full rounded-lg" src={img} alt="" />
  </div>
  <div class="my-4 text-xl font-bold">使用须知</div>
  <p class="indent-4">
    本软件仅用于学习开发测试，不得用于任何商业用途，请在下载之时起24小时之内删除。
  </p>
  <p class="indent-4">
    您可以自由选择是否使用本软件。如果您下载、安装、使用本软件，即表明您信任该软件作者，对任何原因在使用本软件时可能对您自己或他人造成的任何形式的损失不承担责任。
  </p>
  <div class="flex justify-center gap-4 mt-4">
    <Button onClick={onCountinue} {disabled}>
      我同意并继续
      {#if count > 0}
        ({count}s)
      {/if}
    </Button>
    <Button type="normal" onClick={onDisagree}>不同意并退出</Button>
  </div>
</div>
