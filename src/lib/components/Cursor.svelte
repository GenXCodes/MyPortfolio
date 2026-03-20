<script>
  import { onMount } from 'svelte'

  let x = $state(0)
  let y = $state(0)
  let rx = $state(0)
  let ry = $state(0)
  let hovered = $state(false)

  onMount(() => {
    let rafId
    let tx = 0, ty = 0

    const onMove = (e) => {
      x = e.clientX
      y = e.clientY
      hovered = e.target.matches('a, button, [role="tab"], input, textarea, label')
    }

    const animRing = () => {
      rx += (x - rx) * 0.12
      ry += (y - ry) * 0.12
      rafId = requestAnimationFrame(animRing)
    }

    window.addEventListener('mousemove', onMove)
    animRing()

    return () => {
      window.removeEventListener('mousemove', onMove)
      cancelAnimationFrame(rafId)
    }
  })
</script>

<!-- Cursor dot -->
<div
  class="cursor-dot"
  class:hovered
  style="transform: translate({x - 6}px, {y - 6}px)"
></div>

<!-- Cursor ring (lagged) -->
<div
  class="cursor-ring"
  class:hovered
  style="transform: translate({rx - 18}px, {ry - 18}px)"
></div>

<style>
  .cursor-dot {
    position: fixed;
    top: 0; left: 0;
    width: 12px; height: 12px;
    background: var(--accent);
    border-radius: 50%;
    pointer-events: none;
    z-index: 9999;
    mix-blend-mode: screen;
    transition: width 0.2s, height 0.2s, background 0.2s, transform 0.05s linear;
    will-change: transform;
  }

  .cursor-dot.hovered {
    width: 6px; height: 6px;
    background: var(--accent2);
  }

  .cursor-ring {
    position: fixed;
    top: 0; left: 0;
    width: 36px; height: 36px;
    border: 1px solid rgba(0,200,255,0.45);
    border-radius: 50%;
    pointer-events: none;
    z-index: 9998;
    will-change: transform;
    transition: width 0.3s, height 0.3s, border-color 0.3s;
  }

  .cursor-ring.hovered {
    width: 50px; height: 50px;
    border-color: rgba(0,255,157,0.45);
  }
</style>
