<script lang="ts">
  import { isEmpty } from 'lodash';
  import { random } from '../utils';
  import axios from 'axios';
  import bgPreview from '../assets/bg-preview.png';
  import Button from '../components/Button/index.svelte';
  import Input from '../components/Input/index.svelte';
  import Select from '../components/Select/index.svelte';

  const sourceOptions = [
    { label: '官方', value: 'official' },
    { label: '必应', value: 'bing' },
    { label: '360', value: '360' },
    { label: '自定', value: 'fixed' }
  ];

  const getModeOptions = [
    { label: '每日壁纸', value: 'everyday' },
    { label: '每次8张随机', value: 'everytime' }
  ];

  // 壁纸来源
  let source = 'official';
  // bing获取逻辑
  let getMode = 'everyday';
  // 360壁纸分类选项
  let classifyOptions = [];
  // 360壁纸分类
  let classify = '';
  // 预览图地址
  let previewSrc = 'https://oss.ugreen.cloud/ugreen/web-bg/web-bg.png';

  $: {
    if (previewSrc) {
      const img: any = document.getElementById('bg-preview');
      if (img) {
        img.src = previewSrc;
      }
    }
  }

  // 壁纸来源变更
  const onSourceChange = (_, v: string) => {
    source = v;
    switch (v) {
      case 'official':
        previewSrc = 'https://oss.ugreen.cloud/ugreen/web-bg/web-bg.png';
        break;
      case 'bing':
        getBingBg(1);
        break;
      case '360':
        get360Classify();
        getMode = 'everytime';
        break;
      case 'fixed':
        previewSrc = '';
        break;
      default:
        break;
    }
  };

  // bing获取模式变更
  const onGetModeChange = (_, v: string) => {
    getMode = v;
    if (source === 'bing') {
      if (v === 'everyday') {
        getBingBg(1);
      } else {
        getBingBg(8);
      }
    }
  };

  // 获取bing壁纸地址
  const getBingBg = (n: number) => {
    axios
      .get(
        `https://cn.bing.com/HPImageArchive.aspx?format=js&idx=0&n=${n}&mkt=zh-CN`
      )
      .then(({ data }: any) => {
        // 在 8 张图片里面随机取一张，如果上面 n 改了值，下面的 8 也要改成对应的数值，防止取不到图片值而失效
        const s = 'https://cn.bing.com' + data.images[random(0, n - 1)].url;
        previewSrc = s;
      })
      .catch(() => {});
  };

  // 获取360分类
  const get360Classify = () => {
    axios
      .get(
        'http://cdn.apc.360.cn/index.php?c=WallPaper&a=getAllCategoriesV2&from=360chrome'
      )
      .then(({ data }: any) => {
        classifyOptions = data.data.map((o: any) => ({
          label: o.name,
          value: `${o.id}_${o.order_num}`
        }));
        classify = classifyOptions[0]?.value || '';
        get360Bg(classify);
      });
  };

  // 360分类变更
  const onClassifyChange = (_, v: string) => {
    get360Bg(v);
  };

  // 选定360壁纸分类
  const get360Bg = (v: string) => {
    const classId = v.split('_')[0];
    const orders = Number(v.split('_')[1]);
    axios
      .get(
        `http://wallpaper.apc.360.cn/index.php?c=WallPaper&a=getAppsByCategory&cid=${classId}&start=${random(
          1,
          orders
        )}&count=8&from=360chrome`
      )
      .then(({ data }: any) => {
        const bgList = data.data;
        if (!isEmpty(bgList)) {
          previewSrc = bgList[random(0, bgList.length - 1)].url;
        }
      });
  };
</script>

<div class="grid grid-cols-2 gap-8">
  <div class="">
    <Select
      label="壁纸来源"
      options={sourceOptions}
      onChange={onSourceChange}
    />
    {#if source === '360'}
      <Select
        label="选择分类"
        options={classifyOptions}
        onChange={onClassifyChange}
      />
    {/if}
    {#if ['360', 'bing'].includes(source)}
      <Select
        label="获取模式"
        disabled={source === '360'}
        defaultValue={getMode}
        options={getModeOptions}
        onChange={onGetModeChange}
      />
    {/if}
    {#if source === 'bing' && getMode === 'everyday'}
      <div class="text-xs text-slate-400">每日更换，当天壁纸为同一张</div>
    {/if}
    {#if ['360', 'bing'].includes(source) && getMode === 'everytime'}
      <div class="text-xs text-slate-400">
        {#if source === '360'}
          (暂不支持每日壁纸)
        {/if}
        每次获取 8 张壁纸随机取一张
      </div>
    {/if}
    {#if source === 'fixed'}
      <Input label="自定义地址" />
    {/if}
  </div>
  <div>
    <div
      class="relative flex justify-center mt-6 overflow-hidden rounded-sm w-80"
    >
      <img src={bgPreview} alt="" class="z-10 w-80 bg-preview" />
      <img
        id="bg-preview"
        src={previewSrc}
        class="absolute top-0 w-80 bg-preview"
        alt=""
      />
    </div>
    <div class="mt-2 text-center w-80 text-slate-300">示意图仅供参考</div>
  </div>
</div>
<div class="flex justify-center mt-6">
  <Button disabled={source === 'official'}>保存设置</Button>
</div>

<style>
  .bg-preview {
    height: 187px;
  }
</style>
