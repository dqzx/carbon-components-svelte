<script>
  /** Set to `false` to hide the side nav by default */
  export let expandedByDefault = true;

  /** Set to `true` to open the side nav */
  export let isSideNavOpen = false;

  /**
   * Specify the ARIA label for the header
   * @type {string}
   */
  export let uiShellAriaLabel = undefined;

  /**
   * Specify the `href` attribute
   * @type {string}
   */
  export let href = undefined;

  /**
   * Specify the company name
   * @type {string}
   */
  export let company = undefined;

  /**
   * Specify the platform name
   * Alternatively, use the named slot "platform" (e.g., <span slot="platform">...</span>)
   */
  export let platformName = "";

  /** Set to `true` to persist the hamburger menu */
  export let persistentHamburgerMenu = false;

  /** Obtain a reference to the HTML anchor element */
  export let ref = null;

  /**
   * Specify the icon from `carbon-icons-svelte` to render for the closed state
   * Defaults to `Menu20`
   * @type {typeof import("carbon-icons-svelte").CarbonIcon}
   */
  export let iconMenu = Menu20;

  /**
   * Specify the icon from `carbon-icons-svelte` to render for the opened state
   * Defaults to `Close20`
   * @type {typeof import("carbon-icons-svelte").CarbonIcon}
   */
  export let iconClose = Close20;

  import Close20 from "carbon-icons-svelte/lib/Close20/Close20.svelte";
  import Menu20 from "carbon-icons-svelte/lib/Menu20/Menu20.svelte";
  import { shouldRenderHamburgerMenu } from "../navStore";
  import HamburgerMenu from "../SideNav/HamburgerMenu.svelte";

  let winWidth = undefined;

  $: isSideNavOpen =
    expandedByDefault && winWidth >= 1056 && !persistentHamburgerMenu;
  $: ariaLabel = company
    ? `${company} `
    : "" + (uiShellAriaLabel || $$props["aria-label"] || platformName);
</script>

<svelte:window bind:innerWidth="{winWidth}" />

<header role="banner" aria-label="{ariaLabel}" class:bx--header="{true}">
  <slot name="skip-to-content" />
  {#if ($shouldRenderHamburgerMenu && winWidth < 1056) || persistentHamburgerMenu}
    <HamburgerMenu
      bind:isOpen="{isSideNavOpen}"
      iconClose="{iconClose}"
      iconMenu="{iconMenu}"
    />
  {/if}
  <a
    href="{href}"
    class:bx--header__name="{true}"
    bind:this="{ref}"
    {...$$restProps}
    on:click
  >
    {#if company}
      <span class:bx--header__name--prefix="{true}">{company}&nbsp;</span>
    {/if}
    <slot name="platform">{platformName}</slot>
  </a>
  <slot />
</header>
