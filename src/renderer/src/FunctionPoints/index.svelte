<script lang="ts">
  import ugreenLogo from '../assets/ugreen.png';
  import githubLogo from '../assets/github.png';
  import img1 from '../assets/undraw_re_f1pk.svg';
  import img2 from '../assets/undraw_re_xani.svg';
  import img3 from '../assets/undraw_re_5ahy.svg';
</script>

<div class="grid grid-cols-3 gap-8 mb-14 text-slate-50">
  <div
    class="flex flex-col items-center px-10 py-8 shadow-xl cursor-pointer select-none rounded-xl bg-slate-900 hover-scale"
  >
    <div class="relative w-32 h-20">
      <img class="w-full h-full rounded-lg" src={img1} alt="" />
    </div>
    <div class="mt-8 mb-2 text-lg font-bold">自定义壁纸</div>
    <div class="text-xs text-slate-300">
      可以自定义为您喜欢或第三方的随机壁纸
    </div>
  </div>
  <div
    class="flex flex-col items-center px-10 py-8 shadow-xl cursor-pointer select-none rounded-xl bg-slate-900 hover-scale"
  >
    <div class="relative w-32 h-20">
      <img class="w-full h-full rounded-lg" src={img2} alt="" />
    </div>
    <div class="mt-8 mb-2 text-lg font-bold">自定义导航图标</div>
    <div class="text-xs text-slate-300">可以自定义导航的样式和桌面图标</div>
  </div>
  <div
    class="flex flex-col items-center px-10 py-8 shadow-xl cursor-pointer select-none rounded-xl bg-slate-900 hover-scale"
  >
    <div class="relative w-32 h-20">
      <img class="w-full h-full rounded-lg" src={img3} alt="" />
    </div>
    <div class="mt-8 mb-2 text-lg font-bold">高级功能</div>
    <div class="text-xs text-slate-300">建设中，敬请期待</div>
  </div>
</div>
<div class="grid grid-cols-2 gap-8">
  <div
    class="flex items-center justify-between h-20 pl-6 pr-4 overflow-hidden shadow-xl cursor-pointer select-none rounded-xl hover-scale sm-block"
  >
    <div class="z-10 font-bold text-slate-300">官方软件下载地址</div>
    <img class="z-10 w-32" src={ugreenLogo} alt="" />
  </div>
  <div
    class="flex items-center justify-between h-20 pl-6 pr-4 overflow-hidden shadow-xl cursor-pointer select-none rounded-xl hover-scale sm-block"
  >
    <div class="z-10 font-bold text-slate-300">本开源项目地址</div>
    <img class="z-10 w-24" src={githubLogo} alt="" />
  </div>
</div>

<style>
  .sm-block {
    background-color: #490a3d;
    background-image: linear-gradient(
      -25deg,
      #dbdcd7 0%,
      #dddcd7 24%,
      #e2c9cc 30%,
      #e7627d 46%,
      #b8235a 59%,
      #801357 71%,
      #3d1635 84%,
      #1c1a27 100%
    );
  }

  .hover-scale:hover {
    transform: scale(1.03);
    transition: all 0.2s linear;
    opacity: 0.9;
  }
</style>
