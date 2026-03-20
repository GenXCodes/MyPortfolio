<script>
  let scrolled = $state(false)
  let activeSection = $state('home')
  let menuOpen = $state(false)

  const links = [
    { id: 'about', label: '// about' },
    { id: 'projects', label: '// projects' },
    { id: 'skills', label: '// skills' },
    { id: 'contact', label: '// contact' },
  ]

  $effect(() => {
    const onScroll = () => {
      scrolled = window.scrollY > 50
      const sections = ['home','about','projects','skills','contact']
      for (const id of sections.reverse()) {
        const el = document.getElementById(id)
        if (el && window.scrollY >= el.offsetTop - 120) {
          activeSection = id
          break
        }
      }
    }
    window.addEventListener('scroll', onScroll)
    return () => window.removeEventListener('scroll', onScroll)
  })

  function scrollTo(id) {
    document.getElementById(id)?.scrollIntoView({ behavior: 'smooth' })
    menuOpen = false
  }
</script>

<nav class:scrolled>
  <button class="logo" onclick={() => scrollTo('home')} aria-label="Go to top">
    <span class="logo-bracket">[</span>
    <span class="logo-name">RECKIE</span>
    <span class="logo-bracket">]</span>
  </button>

  <ul class="nav-links" class:open={menuOpen}>
    {#each links as link}
      <li>
        <button
          class:active={activeSection === link.id}
          onclick={() => scrollTo(link.id)}
        >
          {link.label}
        </button>
      </li>
    {/each}
    <li class="nav-cta">
      <button class="hire-btn" onclick={() => scrollTo('contact')}>
        <span>Hire Me</span>
      </button>
    </li>
  </ul>

  <button class="hamburger" onclick={() => menuOpen = !menuOpen} aria-label="menu">
    <span class:open={menuOpen}></span>
    <span class:open={menuOpen}></span>
    <span class:open={menuOpen}></span>
  </button>
</nav>

<style>
  nav {
    position: fixed;
    top: 0; left: 0; right: 0;
    z-index: 500;
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 1.4rem 4rem;
    transition: all 0.3s ease;
    border-bottom: 1px solid transparent;
  }

  nav.scrolled {
    background: rgba(3, 8, 13, 0.92);
    backdrop-filter: blur(16px);
    border-bottom-color: var(--border);
    padding: 1rem 4rem;
  }

  .logo {
    font-family: var(--display);
    font-size: 1rem;
    font-weight: 700;
    letter-spacing: 0.05em;
    cursor: pointer;
    color: var(--text-bright);
    display: flex;
    gap: 2px;
    background: none;
    border: none;
    padding: 0;
  }

  .logo-bracket { color: var(--accent); }
  .logo-name { color: var(--text-bright); }

  .nav-links {
    display: flex;
    align-items: center;
    gap: 0.25rem;
    list-style: none;
  }

  .nav-links button {
    background: none;
    border: none;
    font-family: var(--mono);
    font-size: 0.78rem;
    color: var(--text-dim);
    cursor: none;
    padding: 0.5rem 0.9rem;
    border-radius: 4px;
    transition: color 0.2s, background 0.2s;
    letter-spacing: 0.02em;
  }

  .nav-links button:hover,
  .nav-links button.active {
    color: var(--accent);
    background: rgba(0,200,255,0.07);
  }

  .hire-btn {
    background: transparent !important;
    border: 1px solid var(--accent) !important;
    color: var(--accent) !important;
    padding: 0.45rem 1.1rem !important;
    position: relative;
    overflow: hidden;
  }

  .hire-btn::before {
    content: '';
    position: absolute;
    inset: 0;
    background: var(--accent);
    transform: translateX(-100%);
    transition: transform 0.3s ease;
  }

  .hire-btn:hover::before { transform: translateX(0); }
  .hire-btn span { position: relative; z-index: 1; }
  .hire-btn:hover span { color: var(--bg) !important; }

  .hamburger {
    display: none;
    flex-direction: column;
    gap: 5px;
    background: none;
    border: none;
    cursor: none;
    padding: 4px;
  }

  .hamburger span {
    display: block;
    width: 24px;
    height: 1.5px;
    background: var(--text);
    transition: all 0.3s;
    transform-origin: center;
  }

  .hamburger span.open:nth-child(1) { transform: translateY(6.5px) rotate(45deg); }
  .hamburger span.open:nth-child(2) { opacity: 0; }
  .hamburger span.open:nth-child(3) { transform: translateY(-6.5px) rotate(-45deg); }

  @media (max-width: 768px) {
    nav { padding: 1.2rem 1.5rem; }
    nav.scrolled { padding: 1rem 1.5rem; }
    .hamburger { display: flex; }
    .nav-links {
      display: none;
      position: absolute;
      top: 100%; left: 0; right: 0;
      background: rgba(3, 8, 13, 0.98);
      flex-direction: column;
      padding: 1.5rem;
      border-bottom: 1px solid var(--border);
      gap: 0.5rem;
    }
    .nav-links.open { display: flex; }
    .nav-links button { width: 100%; text-align: left; padding: 0.75rem 1rem; }
  }
</style>
