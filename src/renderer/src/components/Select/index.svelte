<script lang="ts">
  import { slide } from 'svelte/transition';
  import { isEmpty } from 'lodash';
  import cx from 'classnames';

  export let label: string = 'Label';
  export let required: boolean = false;
  export let disabled: boolean = false;
  export let defaultValue: string | number = '';
  export let options: any[] = [];
  export let onChange: (e: Event, value: string) => void = () => {};

  let value: string | number = '';
  let visible: boolean = false;

  $: {
    if (defaultValue && isEmpty(value)) {
      value = defaultValue;
    }
    if (!value && !isEmpty(options)) {
      value = options[0].value;
    }

    if (visible) {
      window.setTimeout(() => {
        document.addEventListener('click', closeDropdown);
      });
    } else {
      document.removeEventListener('click', closeDropdown);
    }
  }

  const openDropdown = () => {
    visible = true;
  };

  const closeDropdown = () => {
    visible = false;
  };

  const handleSelect = (v: string, e: Event) => {
    value = v;
    visible = false;
    // @ts-ignore
    e.target.value = v;
    onChange(e, v);
  };
</script>

<div class="pt-1 pb-2 rr-select">
  {#if label}
    <div class="mb-2 leading-4 text-slate-50 rr-input-label">
      {label}
      {#if required}
        <span class="text-red-500">*</span>
      {/if}
    </div>
  {/if}
  <div class="relative">
    <!-- svelte-ignore a11y-click-events-have-key-events -->
    <div
      class={cx(
        'box-border flex items-center h-10 px-2 border-2 rounded cursor-pointer select-none border-slate-900 bg-slate-900',
        disabled &&
          '!cursor-not-allowed text-slate-500 bg-slate-800 border-slate-800'
      )}
      on:click={disabled ? () => {} : openDropdown}
    >
      <div class="flex-1 leading-8 text-left truncate">
        {options.find((o) => o.value === value)?.label || ''}
      </div>
      <span class="ml-1">
        <svg
          class={cx('w-4 h-4 ', visible ? 'rotate-90' : '-rotate-90')}
          viewBox="0 0 1024 1024"
          version="1.1"
          fill="currentColor"
          xmlns="http://www.w3.org/2000/svg"
          width="32"
          height="32"
        >
          <path
            d="M589.088 790.624L310.464 512l278.624-278.624 45.248 45.248L400.96 512l233.376 233.376z"
          />
        </svg>
      </span>
    </div>
    {#if visible}
      <div
        class="box-border absolute left-0 z-10 w-full p-1 overflow-y-auto rounded shadow-lg bg-slate-900 _gk-select-dropdown top-10 max-h-48"
        transition:slide={{ duration: 200 }}
      >
        {#each options as item}
          <!-- svelte-ignore a11y-click-events-have-key-events -->
          <div
            class={cx(
              'py-1 px-2 text-left truncate rounded cursor-pointer hover:bg-slate-600 h-8',
              value === item.value && 'font-bold bg-slate-700'
            )}
            on:click={(e) => handleSelect(item.value, e)}
          >
            {item.label}
          </div>
        {/each}
      </div>
    {/if}
  </div>
</div>
