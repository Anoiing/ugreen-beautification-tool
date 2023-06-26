<script lang="ts">
  import cx from 'classnames';
  export let type: string = 'primary';
  export let disabled: boolean = false;
  export let onClick: () => void = () => {};
</script>

<div
  class={cx(
    'pointer-events-auto rounded-md px-3 inline-flex items-center leading-6 h-9',
    type === 'primary' && 'bg-violet-900 hover:bg-violet-800 text-white',
    type === 'normal' &&
      ' bg-slate-500 border-2 border-gray-600 hover:border-violet-900 text-gray-300 hover:text-violet-100',
    disabled
      ? 'cursor-not-allowed !bg-slate-500 !hover:bg-slate-500'
      : 'cursor-pointer'
  )}
  on:click={disabled ? () => {} : onClick}
  on:keypress
>
  <slot><!-- optional fallback --></slot>
</div>
