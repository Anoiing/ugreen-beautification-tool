<script lang="ts">
  import Icon from '../components/Icon/index.svelte';
  import macosIcon from '../assets/macos.svg';
  import niceTry from 'nice-try';
  import windowsIcon from '../assets/windows.svg';

  let fileName = '';

  const onChange = (e: Event) => {
    console.log(e);
    // @ts-ignore
    fileName = niceTry(() => e.target!.files[0]?.path);
  };
</script>

<div class="px-4">
  <div>请查找 <span class="text-lg font-bold">app.asar</span> 文件并上传</div>
  <div class="mt-4 mb-2 text-slate-300">查找位置参考：</div>
  <div class="mb-8 text-sm text-slate-300">
    <div class="flex items-center gap-2 mb-2 indent-2">
      <img class="w-4 h-4" src={windowsIcon} alt="" />
      Windows平台：[安装盘符]:\ProgramFiles (x86)\UGREEN_Nas\resources
    </div>
    <div class="flex items-center gap-2 indent-2">
      <img class="w-4 h-4" src={macosIcon} alt="" />
      MacOS平台：/Applications/绿联云.app/Contents/Resources
    </div>
  </div>
  <div class="relative h-40 mb-8 rounded-lg select-none bg-slate-900 w-96">
    <div
      class="absolute top-0 left-0 flex flex-col items-center justify-center w-full h-full px-8 border border-dashed rounded-lg border-slate-50 text-slate-100"
    >
      {#if fileName}
        <Icon name="file" />
        <p class="mt-2 text-xs">{fileName}</p>
      {:else}
        <Icon name="upload" />
        <p class="mt-2 text-xs">拖拽至此进行上传</p>
      {/if}
    </div>
    <input
      type="file"
      name="asar_file"
      class="absolute top-0 left-0 w-full h-full opacity-0 cursor-pointer"
      on:change={onChange}
    />
  </div>
  <div>
    {fileName}
  </div>
</div>
