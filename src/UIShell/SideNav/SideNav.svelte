<script>
  /**
   * @event {any} open
   * @event {any} close
   * @event {any} click:overlay
   */

  /** Set to `true` to use the fixed variant */
  export let fixed = false;

  /** Set to `true` to use the rail variant */
  export let rail = false;

  /**
   * Specify the ARIA label for the nav
   * @type {string}
   */
  export let ariaLabel = undefined;

  /** Set to `true` to toggle the expanded state */
  export let isOpen = false;

  import { onMount, createEventDispatcher } from "svelte";
  import { shouldRenderHamburgerMenu } from "../navStore";

  const dispatch = createEventDispatcher();

  let winWidth = undefined;

  $: dispatch(isOpen ? "open" : "close");

  onMount(() => {
    shouldRenderHamburgerMenu.set(true);
    return () => shouldRenderHamburgerMenu.set(false);
  });
</script>

<svelte:window bind:innerWidth="{winWidth}" />

{#if !fixed}
  <div
    on:click="{() => {
      dispatch('click:overlay');
      isOpen = false;
    }}"
    class:bx--side-nav__overlay="{true}"
    class:bx--side-nav__overlay-active="{isOpen}"
  ></div>
{/if}
<nav
  aria-hidden="{!isOpen}"
  aria-label="{ariaLabel}"
  class:bx--side-nav__navigation="{true}"
  class:bx--side-nav="{true}"
  class:bx--side-nav--ux="{true}"
  class:bx--side-nav--expanded="{rail && winWidth >= 1056 ? false : isOpen}"
  class:bx--side-nav--collapsed="{!isOpen && !rail}"
  class:bx--side-nav--rail="{rail}"
  {...$$restProps}
>
  <slot />
</nav>
