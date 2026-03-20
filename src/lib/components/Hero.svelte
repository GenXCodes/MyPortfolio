<script>
  import { onMount } from 'svelte'

  let canvas = $state(null)
  let typedText = $state('')
  let cursorVisible = $state(true)
  let statusLine = $state(0)

  const roles = [
    'Web Developer',
    'MSP Support Engineer',
    'AI Automation Specialist',
    'Technical Support Pro',
    'Full-Stack Builder',
  ]

  const statusLines = [
    '> Initializing portfolio...',
    '> Loading skill modules...',
    '> Establishing connection...',
    '> System ready.',
  ]

  let roleIndex = $state(0)
  let charIndex = $state(0)
  let deleting = $state(false)

  onMount(() => {
    // Typewriter
    let timeout
    const type = () => {
      const current = roles[roleIndex]
      if (!deleting) {
        typedText = current.slice(0, charIndex + 1)
        charIndex++
        if (charIndex === current.length) {
          setTimeout(() => { deleting = true; loop() }, 2200)
          return
        }
      } else {
        typedText = current.slice(0, charIndex - 1)
        charIndex--
        if (charIndex === 0) {
          deleting = false
          roleIndex = (roleIndex + 1) % roles.length
        }
      }
      timeout = setTimeout(loop, deleting ? 45 : 80)
    }
    const loop = () => { timeout = setTimeout(type, 0) }
    loop()

    // Cursor blink
    const cursorInterval = setInterval(() => {
      cursorVisible = !cursorVisible
    }, 530)

    // Status lines animation
    let i = 0
    const statusInterval = setInterval(() => {
      statusLine = ++i
      if (i >= statusLines.length) clearInterval(statusInterval)
    }, 400)

    // Particle canvas
    const c = canvas
    if (!c) return
    const ctx = c.getContext('2d')
    let W, H, particles = [], animId

    const resize = () => {
      W = c.width = c.offsetWidth
      H = c.height = c.offsetHeight
    }
    resize()
    window.addEventListener('resize', resize)

    const makeParticle = () => {
      let p = { x:0, y:0, size:0, speedX:0, speedY:0, opacity:0, color:'' }
      const reset = () => {
        p.x = Math.random() * W
        p.y = Math.random() * H
        p.size = Math.random() * 1.5 + 0.3
        p.speedX = (Math.random() - 0.5) * 0.4
        p.speedY = (Math.random() - 0.5) * 0.4
        p.opacity = Math.random() * 0.5 + 0.1
        p.color = Math.random() > 0.6 ? '#00c8ff' : Math.random() > 0.5 ? '#00ff9d' : '#ffffff'
      }
      p.update = () => {
        p.x += p.speedX; p.y += p.speedY
        if (p.x < 0 || p.x > W || p.y < 0 || p.y > H) reset()
      }
      p.draw = () => {
        ctx.save()
        ctx.globalAlpha = p.opacity
        ctx.fillStyle = p.color
        ctx.shadowBlur = 6
        ctx.shadowColor = p.color
        ctx.beginPath()
        ctx.arc(p.x, p.y, p.size, 0, Math.PI * 2)
        ctx.fill()
        ctx.restore()
      }
      reset()
      return p
    }

    for (let j = 0; j < 120; j++) particles.push(makeParticle())

    const drawConnections = () => {
      for (let a = 0; a < particles.length; a++) {
        for (let b = a + 1; b < particles.length; b++) {
          const dx = particles[a].x - particles[b].x
          const dy = particles[a].y - particles[b].y
          const dist = Math.sqrt(dx*dx + dy*dy)
          if (dist < 90) {
            ctx.save()
            ctx.globalAlpha = (1 - dist / 90) * 0.12
            ctx.strokeStyle = '#00c8ff'
            ctx.lineWidth = 0.5
            ctx.beginPath()
            ctx.moveTo(particles[a].x, particles[a].y)
            ctx.lineTo(particles[b].x, particles[b].y)
            ctx.stroke()
            ctx.restore()
          }
        }
      }
    }

    const animate = () => {
      ctx.clearRect(0, 0, W, H)
      drawConnections()
      particles.forEach(p => { p.update(); p.draw() })
      animId = requestAnimationFrame(animate)
    }
    animate()

    return () => {
      clearTimeout(timeout)
      clearInterval(cursorInterval)
      cancelAnimationFrame(animId)
      window.removeEventListener('resize', resize)
    }
  })

  function scrollToAbout() {
    document.getElementById('about')?.scrollIntoView({ behavior: 'smooth' })
  }
</script>

<section id="home">
  <canvas bind:this={canvas}></canvas>

  <div class="hero-content">
    <!-- Terminal window -->
    <div class="terminal" aria-hidden="true">
      <div class="terminal-bar">
        <span class="dot red"></span>
        <span class="dot yellow"></span>
        <span class="dot green"></span>
        <span class="terminal-title">portfolio.sh — zsh</span>
      </div>
      <div class="terminal-body">
        {#each statusLines as line, i}
          {#if i < statusLine}
            <div class="t-line" style="animation-delay:{i*0.1}s">
              <span class="prompt">$</span> {line}
            </div>
          {/if}
        {/each}
      </div>
    </div>

    <div class="hero-main">
      <p class="greeting">
        <span class="tag">&lt;</span>Hello World<span class="tag">/&gt;</span>
      </p>

      <h1 class="hero-name">
        YOUR
        <span class="name-accent">NAME</span>
        <span class="name-dot">.</span>
      </h1>

      <div class="role-line">
        <span class="role-prefix">const role = "</span>
        <span class="typed">{typedText}</span>
        <span class="cursor" class:visible={cursorVisible}>|</span>
        <span class="role-suffix">"</span>
      </div>

      <p class="hero-desc">
        Building digital infrastructure, automating workflows, and engineering
        solutions across web, cloud, and AI ecosystems.
      </p>

      <div class="hero-actions">
        <button class="btn-primary" onclick={() => document.getElementById('projects')?.scrollIntoView({behavior:'smooth'})}>
          <span class="btn-icon">▶</span> View Projects
        </button>
        <button class="btn-secondary" onclick={() => document.getElementById('contact')?.scrollIntoView({behavior:'smooth'})}>
          <span>Get In Touch</span>
          <span class="btn-arrow">→</span>
        </button>
      </div>

      <div class="hero-stats">
        <div class="stat">
          <span class="stat-num">3+</span>
          <span class="stat-label">Years Exp</span>
        </div>
        <div class="stat-divider"></div>
        <div class="stat">
          <span class="stat-num">20+</span>
          <span class="stat-label">Projects Done</span>
        </div>
        <div class="stat-divider"></div>
        <div class="stat">
          <span class="stat-num">100%</span>
          <span class="stat-label">Client Satisfaction</span>
        </div>
      </div>
    </div>
  </div>

  <!-- Scroll indicator -->
  <button class="scroll-indicator" onclick={scrollToAbout} aria-label="Scroll down">
    <span class="scroll-label">scroll</span>
    <div class="scroll-line">
      <div class="scroll-dot"></div>
    </div>
  </button>

  <!-- Corner decorations -->
  <div class="corner tl"></div>
  <div class="corner tr"></div>
  <div class="corner bl"></div>
  <div class="corner br"></div>
</section>

<style>
  section {
    position: relative;
    min-height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
    overflow: hidden;
    padding: 6rem 2rem 4rem;
  }

  canvas {
    position: absolute;
    inset: 0;
    width: 100%;
    height: 100%;
    opacity: 0.6;
  }

  /* Radial gradient overlay */
  section::before {
    content: '';
    position: absolute;
    inset: 0;
    background:
      radial-gradient(ellipse 80% 60% at 20% 50%, rgba(0,200,255,0.06) 0%, transparent 60%),
      radial-gradient(ellipse 60% 80% at 80% 20%, rgba(0,255,157,0.04) 0%, transparent 60%);
    pointer-events: none;
  }

  .hero-content {
    position: relative;
    z-index: 2;
    display: flex;
    gap: 3rem;
    align-items: center;
    max-width: 1100px;
    width: 100%;
  }

  /* Terminal */
  .terminal {
    width: 300px;
    flex-shrink: 0;
    background: rgba(6, 15, 23, 0.9);
    border: 1px solid var(--border-bright);
    border-radius: 8px;
    overflow: hidden;
    box-shadow: 0 0 40px rgba(0,200,255,0.08), inset 0 1px 0 rgba(255,255,255,0.05);
    animation: fadeSlideLeft 0.8s ease both;
  }

  .terminal-bar {
    display: flex;
    align-items: center;
    gap: 6px;
    padding: 0.6rem 1rem;
    background: rgba(255,255,255,0.04);
    border-bottom: 1px solid var(--border);
  }

  .dot {
    width: 10px; height: 10px;
    border-radius: 50%;
  }
  .dot.red { background: #ff5f57; }
  .dot.yellow { background: #febc2e; }
  .dot.green { background: #28c840; }

  .terminal-title {
    font-family: var(--mono);
    font-size: 0.65rem;
    color: var(--text-dim);
    margin-left: auto;
  }

  .terminal-body {
    padding: 1rem;
    min-height: 120px;
  }

  .t-line {
    font-family: var(--mono);
    font-size: 0.72rem;
    color: var(--text);
    margin-bottom: 0.4rem;
    opacity: 0;
    animation: termLine 0.3s ease forwards;
  }

  .prompt {
    color: var(--accent2);
    margin-right: 0.5rem;
  }

  /* Hero main */
  .hero-main {
    flex: 1;
    animation: fadeSlideRight 0.8s 0.2s ease both;
  }

  .greeting {
    font-family: var(--mono);
    font-size: 0.85rem;
    color: var(--text-dim);
    margin-bottom: 0.75rem;
    letter-spacing: 0.05em;
  }

  .tag { color: var(--accent); }

  .hero-name {
    font-family: var(--display);
    font-size: clamp(3rem, 7vw, 5.5rem);
    font-weight: 900;
    line-height: 0.95;
    letter-spacing: -0.03em;
    color: var(--text-bright);
    margin-bottom: 1.2rem;
  }

  .name-accent {
    color: transparent;
    -webkit-text-stroke: 2px var(--accent);
    display: block;
  }

  .name-dot { color: var(--accent2); }

  .role-line {
    font-family: var(--mono);
    font-size: clamp(0.85rem, 2vw, 1rem);
    color: var(--text);
    margin-bottom: 1.5rem;
    padding: 0.6rem 1rem;
    background: rgba(0,200,255,0.05);
    border-left: 2px solid var(--accent);
    border-radius: 0 4px 4px 0;
    display: inline-block;
  }

  .role-prefix { color: var(--text-dim); }
  .typed { color: var(--accent2); }
  .cursor {
    color: var(--accent);
    opacity: 0;
    transition: opacity 0.1s;
  }
  .cursor.visible { opacity: 1; }
  .role-suffix { color: var(--text-dim); }

  .hero-desc {
    font-family: var(--body);
    font-size: clamp(0.9rem, 1.8vw, 1.05rem);
    color: var(--text);
    line-height: 1.7;
    max-width: 480px;
    margin-bottom: 2rem;
    font-weight: 300;
  }

  .hero-actions {
    display: flex;
    gap: 1rem;
    flex-wrap: wrap;
    margin-bottom: 2.5rem;
  }

  .btn-primary {
    display: flex;
    align-items: center;
    gap: 0.6rem;
    background: var(--accent);
    color: var(--bg);
    border: none;
    font-family: var(--mono);
    font-size: 0.82rem;
    font-weight: 600;
    padding: 0.85rem 1.8rem;
    border-radius: 4px;
    cursor: none;
    letter-spacing: 0.05em;
    transition: all 0.2s;
    box-shadow: 0 0 20px rgba(0,200,255,0.3);
  }

  .btn-primary:hover {
    background: var(--text-bright);
    box-shadow: 0 0 30px rgba(0,200,255,0.5);
    transform: translateY(-2px);
  }

  .btn-icon { font-size: 0.65rem; }

  .btn-secondary {
    display: flex;
    align-items: center;
    gap: 0.6rem;
    background: transparent;
    color: var(--text-bright);
    border: 1px solid var(--border-bright);
    font-family: var(--mono);
    font-size: 0.82rem;
    padding: 0.85rem 1.8rem;
    border-radius: 4px;
    cursor: none;
    letter-spacing: 0.05em;
    transition: all 0.2s;
  }

  .btn-secondary:hover {
    border-color: var(--accent);
    color: var(--accent);
    box-shadow: 0 0 15px rgba(0,200,255,0.1);
    transform: translateY(-2px);
  }

  .btn-arrow { transition: transform 0.2s; }
  .btn-secondary:hover .btn-arrow { transform: translateX(4px); }

  .hero-stats {
    display: flex;
    align-items: center;
    gap: 1.5rem;
  }

  .stat { display: flex; flex-direction: column; gap: 2px; }

  .stat-num {
    font-family: var(--display);
    font-size: 1.5rem;
    font-weight: 700;
    color: var(--accent);
  }

  .stat-label {
    font-family: var(--mono);
    font-size: 0.65rem;
    color: var(--text-dim);
    letter-spacing: 0.08em;
    text-transform: uppercase;
  }

  .stat-divider {
    width: 1px;
    height: 36px;
    background: var(--border-bright);
  }

  /* Scroll indicator */
  .scroll-indicator {
    position: absolute;
    bottom: 2rem;
    left: 50%;
    transform: translateX(-50%);
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 0.5rem;
    background: none;
    border: none;
    cursor: none;
    animation: fadeIn 1.5s 1s ease both;
  }

  .scroll-label {
    font-family: var(--mono);
    font-size: 0.6rem;
    color: var(--text-dim);
    letter-spacing: 0.2em;
    text-transform: uppercase;
  }

  .scroll-line {
    width: 1px;
    height: 50px;
    background: var(--border-bright);
    position: relative;
    overflow: hidden;
  }

  .scroll-dot {
    position: absolute;
    top: -4px;
    left: -1px;
    width: 3px;
    height: 8px;
    background: var(--accent);
    border-radius: 2px;
    animation: scrollDot 1.5s ease-in-out infinite;
  }

  /* Corner decorations */
  .corner {
    position: absolute;
    width: 20px;
    height: 20px;
    border-color: var(--accent);
    border-style: solid;
    opacity: 0.4;
  }
  .corner.tl { top: 2rem; left: 2rem; border-width: 1px 0 0 1px; }
  .corner.tr { top: 2rem; right: 2rem; border-width: 1px 1px 0 0; }
  .corner.bl { bottom: 2rem; left: 2rem; border-width: 0 0 1px 1px; }
  .corner.br { bottom: 2rem; right: 2rem; border-width: 0 1px 1px 0; }

  /* Animations */
  @keyframes fadeSlideLeft {
    from { opacity: 0; transform: translateX(-40px); }
    to { opacity: 1; transform: translateX(0); }
  }
  @keyframes fadeSlideRight {
    from { opacity: 0; transform: translateX(40px); }
    to { opacity: 1; transform: translateX(0); }
  }
  @keyframes termLine {
    from { opacity: 0; transform: translateX(-8px); }
    to { opacity: 1; transform: translateX(0); }
  }
  @keyframes scrollDot {
    0% { top: -8px; opacity: 1; }
    100% { top: 54px; opacity: 0; }
  }
  @keyframes fadeIn {
    from { opacity: 0; }
    to { opacity: 1; }
  }

  @media (max-width: 900px) {
    .hero-content { flex-direction: column; align-items: flex-start; gap: 2rem; }
    .terminal { width: 100%; max-width: 360px; }
    section { padding: 7rem 1.5rem 5rem; align-items: flex-start; }
  }
</style>
