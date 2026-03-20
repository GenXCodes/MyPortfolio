<script>
  import { onMount } from 'svelte'

  let show = $state(false)

  onMount(() => {
    const onScroll = () => { show = window.scrollY > 500 }
    window.addEventListener('scroll', onScroll, { passive: true })
    return () => window.removeEventListener('scroll', onScroll)
  })

  function toTop() {
    window.scrollTo({ top: 0, behavior: 'smooth' })
  }
</script>

{#if show}
  <button class="btt" onclick={toTop} aria-label="Back to top" title="Back to top">
    <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round">
      <polyline points="18 15 12 9 6 15"/>
    </svg>
  </button>
{/if}

<style>
  .btt {
    position: fixed;
    bottom: 2rem;
    right: 2rem;
    width: 44px;
    height: 44px;
    border-radius: 8px;
    background: var(--surface);
    border: 1px solid var(--border-bright);
    color: var(--accent);
    display: flex;
    align-items: center;
    justify-content: center;
    cursor: none;
    z-index: 400;
    transition: all 0.2s;
    animation: bttIn 0.3s ease;
    box-shadow: 0 4px 20px rgba(0,0,0,0.3);
  }

  .btt:hover {
    background: rgba(0,200,255,0.1);
    border-color: var(--accent);
    box-shadow: 0 0 20px rgba(0,200,255,0.2);
    transform: translateY(-3px);
  }

  @keyframes bttIn {
    from { opacity: 0; transform: translateY(10px); }
    to   { opacity: 1; transform: translateY(0); }
  }
</style>
