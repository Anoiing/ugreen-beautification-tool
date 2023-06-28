<script lang="ts">
  import cx from 'classnames';

  export let label: string = 'Label';
  export let value: string = '';
  export let required: boolean = false;
  export let errorMessage: string = '';
  export let placeholder: string = '';
  export let onFocus: () => void = () => {};
  export let onBlur: () => void = () => {};

  let focused: boolean = false;

  const handleFocus = () => {
    focused = true;
    onFocus();
  };

  const handleBlur = () => {
    focused = false;
    onBlur();
  };
</script>

<div class="flex flex-col pt-1 pb-2 rr-input">
  {#if label}
    <div class="mb-2 leading-4 text-slate-50">
      {label}
      {#if required}
        <span class="text-red-500">*</span>
      {/if}
    </div>
  {/if}
  <div
    class={cx(
      'rounded px-2  border-2 h-10 flex items-center bg-slate-900 border-slate-900',
      errorMessage ? 'border-red-400' : ''
    )}
  >
    <input
      class="w-full leading-8 bg-transparent border-none"
      {value}
      on:focus={handleFocus}
      on:blur={handleBlur}
      on:input
      on:change
      {placeholder}
    />
  </div>
  {#if errorMessage}
    <div class="text-xs text-red-400">
      {errorMessage}
    </div>
  {/if}
</div>

<style>
  .rr-input input:focus-visible {
    border: none !important;
    outline: none;
  }
  .rr-input input::placeholder {
    color: rgb(226 232 240);
  }
</style>
