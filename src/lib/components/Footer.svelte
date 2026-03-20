<script>
  const year = new Date().getFullYear()
</script>

<footer>
  <div class="container">
    <div class="left">
      <span class="logo">
        <span class="bracket">[</span>RECKIE<span class="bracket">]</span>
      </span>
      <p class="tagline">Building systems that just work.</p>
    </div>

    <div class="center">
      <p class="built-with">
        Built with
        <span class="tech">Svelte 5</span>
        <span class="sep">+</span>
        <span class="tech">Vite</span>
        <span class="sep">·</span>
        <span class="mono">v1.0.0</span>
      </p>
    </div>

    <div class="right">
      <p class="copy">© {year} Reckie Degollacion. All rights reserved.</p>
    </div>
  </div>

  <!-- Top border glow -->
</footer>

<style>
  footer {
    position: relative;
    padding: 2rem 4rem;
    border-top: 1px solid var(--border);
    background: var(--bg);
  }

  footer::before {
    content: '';
    position: absolute;
    top: -1px; left: 10%; right: 10%;
    height: 1px;
    background: linear-gradient(90deg, transparent, var(--accent), var(--accent2), transparent);
    opacity: 0.5;
  }

  .container {
    max-width: 1100px;
    margin: 0 auto;
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 1rem;
    flex-wrap: wrap;
  }

  .logo {
    font-family: var(--display);
    font-size: 0.9rem;
    font-weight: 700;
    color: var(--text-bright);
  }

  .bracket { color: var(--accent); }

  .tagline {
    font-family: var(--mono);
    font-size: 0.65rem;
    color: var(--text-dim);
    margin-top: 4px;
    letter-spacing: 0.08em;
  }

  .built-with {
    font-family: var(--mono);
    font-size: 0.7rem;
    color: var(--text-dim);
    display: flex;
    align-items: center;
    gap: 0.4rem;
  }

  .tech {
    color: var(--accent);
    font-weight: 600;
  }

  .sep { color: var(--border-bright); }
  .mono { color: var(--text-dim); }

  .copy {
    font-family: var(--mono);
    font-size: 0.65rem;
    color: var(--text-dim);
    letter-spacing: 0.05em;
    text-align: right;
  }

  @media (max-width: 768px) {
    footer { padding: 2rem 1.5rem; }
    .container { flex-direction: column; text-align: center; }
    .left, .right { text-align: center; }
    .copy { text-align: center; }
  }
</style>
