<script>
  /**
   * Specify the current value
   * @type {number}
   */
  export let value = undefined;

  /** Specify the maximum value */
  export let max = 100;

  /** Specify the label text */
  export let labelText = "";

  /** Set to `true` to visually hide the label text */
  export let hideLabel = false;

  /** Specify the helper text */
  export let helperText = "";

  let id = "ccs-" + Math.random().toString(36);
  let helperId = "ccs-" + Math.random().toString(36);

  $: indeterminate = value === undefined;
  $: capped = value > max ? max : value < 0 ? 0 : value;
</script>

<div
  class:bx--progress-bar="{true}"
  class:bx--progress-bar--indeterminate="{indeterminate}"
  {...$$restProps}
>
  <label
    for="{id}"
    class:bx--progress-bar__label="{true}"
    class:bx--visually-hidden="{hideLabel}"
  >
    <slot name="labelText">
      {labelText}
    </slot>
  </label>
  <div
    role="progressbar"
    id="{id}"
    class:bx--progress-bar__track="{true}"
    aria-valuemin="{indeterminate ? undefined : 0}"
    aria-valuemax="{indeterminate ? undefined : max}"
    aria-valuenow="{indeterminate ? undefined : capped}"
    aria-describedby="{helperText ? helperId : null}"
  >
    <div
      class:bx--progress-bar__bar="{true}"
      style="transform: scaleX({capped / max})"
    ></div>
  </div>
  {#if helperText}
    <div id="{helperId}" class:bx--progress-bar__helper-text="{true}">
      {helperText}
    </div>
  {/if}
</div>
