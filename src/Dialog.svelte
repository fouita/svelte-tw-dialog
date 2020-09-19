<script>
  import XIcon from "./XIcon.svelte";
  import { fade } from "svelte/transition";

  import { onMount } from "svelte";

  export let visible = false;
  export let color = "indigo";
  export let inverted = false;
  export let title = null;
  export let icon = null;
  export let body = "";
  export let closable = true;
  export let filter = true;
  export let position = "middle"; // top middle bottom
  export let align = "center"; // left center right
  let klass = true;
  export { klass as class };
  export let rounded = "md";
  export let w = "";
  export let h = "";
  export let absolute = false;

  export let header = !!title;

  $: bg = color + (!inverted ? "-700" : "-100");
  $: c_color = color + (!inverted ? "-100" : "-700");

  function close() {
    if (closable) {
      visible = false;
    }
  }

  function getLeft(perw) {
    return align == "left" ? 0 : align == "right" ? 100 - perw : 50 - perw / 2;
  }

  function getTop(perh) {
    return position == "top"
      ? 0
      : position == "bottom"
      ? 100 - perh
      : 50 - perh / 2;
  }

  let m_node, c_node, w_width, w_height;

  function setPosition(node) {
    m_node = node || m_node;
    w_width = absolute ? m_node.parentNode.clientWidth : window.innerWidth;
    w_height = absolute ? m_node.parentNode.clientHeight : window.innerHeight;
    let w = w_width,
      nw = m_node.clientWidth,
      perw = (nw / w) * 100,
      left = getLeft(perw),
      h = w_height,
      nh = m_node.clientHeight,
      perh = (nh / h) * 100,
      top = getTop(perh),
      elm_top = top,
      elm_left = left > 1 ? left : 0;

    if (position == "top" && align == "center") {
      elm_top = top - perh;
    } else if (align == "left") {
      elm_left = left - perw;
    } else if (align == "right") {
      elm_left = left + perw;
    } else if (position == "bottom") {
      elm_top = top + perh;
    }

    m_node.style.left = `${elm_left}%`;
    m_node.style.top = `${elm_top}%`;
  }

  function setMargins() {
    let rect = m_node.getBoundingClientRect();

    let mtop = -rect.y;
    let mleft = -rect.x;

    if (absolute) {
      mtop =
        position == "top"
          ? elm_node.clientHeight
          : position == "bottom"
          ? -m_node.parentNode.clientHeight
          : 0;
      mleft =
        align == "left"
          ? elm_node.clientWidth
          : align == "right"
          ? -m_node.parentNode.clientWidth
          : 0;
      mtop =
        position == "middle" && align == "center"
          ? -(m_node.parentNode.clientHeight - elm_node.clientHeight) / 2
          : mtop;
    } else {
      if (position == "top" && align == "center") {
        mtop = rect.height;
      }
      if (align == "left") {
        mleft = rect.width;
      }
    }

    c_node.style.marginLeft = `${mleft}px`;
    c_node.style.marginTop = `${mtop}px`;
    c_node.style.width = `${w_width}px`;
    c_node.style.height = `${w_height}px`;
  }

  function setFilter(node) {
    c_node = node || c_node;
    setTimeout(setMargins);
  }

  onMount(() => {
    window.addEventListener("resize", resize);
  });

  function resize() {
    if (!visible) return;
    setPosition();
    setMargins();
  }

  function translateElm(m = 0) {
    if (!elm_node) return;
    if (position == "top" && align == "center") {
      elm_node.style.transform = `translateY(${100 - m}%)`;
    } else if (align == "left") {
      elm_node.style.transform = `translateX(${100 - m}%)`;
    } else if (align == "right") {
      elm_node.style.transform = `translateX(${m - 100}%)`;
    } else if (position == "bottom") {
      elm_node.style.transform = `translateY(${m - 100}%)`;
    }
  }

  let elm_node;
  function setElmPosition(node) {
    elm_node = node || elm_node;
    setTimeout(translateElm);
  }

  $: if (!visible && closable) {
    translateElm(100);
  }
</script>

<style>
  .tr {
    transition: 0.5s cubic-bezier(0.87, 0, 0.13, 1);
  }
</style>

{#if visible}
  <div
    transition:fade={{ duration: 400 }}
    class="{absolute ? 'absolute' : 'fixed'} z-30 top-0 left-0 flex flex-row w-{w}
    h-{h}"
    use:setPosition>
    {#if filter}
      <div
        class="absolute w-full h-full bg-gray-800 opacity-50 z-40"
        on:click={close}
        use:setFilter />
    {/if}
    <div
      class="relative max-w-full w-full rounded-{rounded}
      overflow-y-auto overflow-x-hidden max-h-full tr {absolute ? '' : 'bg-white shadow-lg'}
      z-40 {klass}"
      use:setElmPosition>
      {#if title}
        <div
          class="w-full font-medium flex text-lg p-3 mr-6 text-{c_color} bg-{bg}
          border-b">
          <slot name="title">
            {#if icon}
              <svelte:component this={icon} class="w-5 h-5 m-1 mr-2" />
            {/if}
            {title}
          </slot>
        </div>
      {/if}
      {#if closable && header}
        <div
          class="absolute top-0 right-0 text-{title ? `${c_color} mt-4` : 'gray-400 mt-2'}
          mr-2 cursor-pointer"
          on:click={close}>
          <XIcon class="w-5 h-5" />
        </div>
      {/if}
      <slot>
        <div class="p-4">{body}</div>
      </slot>
    </div>
  </div>
{/if}
