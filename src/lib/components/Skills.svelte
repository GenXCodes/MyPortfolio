<script>
  import { onMount } from 'svelte'

  let visible = $state(false)
  let sectionEl = $state(null)
  let activeCategory = $state(0)

  onMount(() => {
    const observer = new IntersectionObserver(
      ([entry]) => { if (entry.isIntersecting) visible = true },
      { threshold: 0.15 }
    )
    if (sectionEl) observer.observe(sectionEl)
    return () => observer.disconnect()
  })

  const categories = [
    {
      label: 'Web Development',
      icon: '⬡',
      color: '#00c8ff',
      skills: [
        { name: 'HTML / CSS', level: 95, icon: '🌐' },
        { name: 'JavaScript / TypeScript', level: 88, icon: '⚡' },
        { name: 'Svelte / SvelteKit', level: 85, icon: '🔥' },
        { name: 'React / Next.js', level: 80, icon: '⚛️' },
        { name: 'Node.js / Express', level: 78, icon: '🟢' },
        { name: 'PostgreSQL / Supabase', level: 75, icon: '🗄️' },
        { name: 'TailwindCSS', level: 90, icon: '🎨' },
        { name: 'REST APIs / GraphQL', level: 82, icon: '🔌' },
      ]
    },
    {
      label: 'MSP & IT Support',
      icon: '◈',
      color: '#00ff9d',
      skills: [
        { name: 'Windows Server / AD', level: 90, icon: '🖥️' },
        { name: 'Microsoft 365 Admin', level: 88, icon: '☁️' },
        { name: 'Azure / Entra ID', level: 80, icon: '🔷' },
        { name: 'ConnectWise / Autotask', level: 85, icon: '🎫' },
        { name: 'Networking / VPN', level: 78, icon: '🌐' },
        { name: 'RMM Tools (NinjaOne)', level: 82, icon: '🛡️' },
        { name: 'PowerShell / Scripting', level: 80, icon: '💻' },
        { name: 'SLA Management', level: 92, icon: '📋' },
      ]
    },
    {
      label: 'AI Automations',
      icon: '◎',
      color: '#ff6f3c',
      skills: [
        { name: 'OpenAI / GPT-4o API', level: 88, icon: '🤖' },
        { name: 'N8N / Make (Integromat)', level: 85, icon: '⚙️' },
        { name: 'LangChain / RAG', level: 78, icon: '🔗' },
        { name: 'Zapier Automations', level: 82, icon: '⚡' },
        { name: 'Python / FastAPI', level: 75, icon: '🐍' },
        { name: 'Prompt Engineering', level: 90, icon: '✏️' },
        { name: 'Vector DBs / Pinecone', level: 72, icon: '📦' },
        { name: 'AI Agent Workflows', level: 80, icon: '🕸️' },
      ]
    },
  ]

  const tools = [
    'VS Code', 'Git / GitHub', 'Docker', 'Vercel', 'Cloudflare',
    'Figma', 'Postman', 'Linux', 'Notion', 'Slack', 'Jira',
    'Microsoft 365', 'Azure', 'Supabase', 'Stripe', 'N8N',
  ]

  let current = $derived(categories[activeCategory])
</script>

<section id="skills" bind:this={sectionEl} class:visible>
  <div class="container">
    <p class="section-label">03 / skills</p>
    <h2 class="section-title">Tech Stack<span class="accent">_</span></h2>

    <div class="content">
      <!-- Category tabs -->
      <div class="cat-tabs">
        {#each categories as cat, i}
          <button
            class:active={activeCategory === i}
            style="--cat-color: {cat.color}"
            onclick={() => activeCategory = i}
          >
            <span class="cat-icon">{cat.icon}</span>
            {cat.label}
          </button>
        {/each}
      </div>

      <!-- Skills panel -->
      <div class="skills-panel">
        <div class="skill-list">
          {#each current.skills as skill, i}
            <div class="skill-row" style="animation-delay: {i * 0.05}s">
              <div class="skill-head">
                <span class="skill-name">
                  <span class="skill-icon">{skill.icon}</span>
                  {skill.name}
                </span>
                <span class="skill-pct" style="color: {current.color}">{skill.level}%</span>
              </div>
              <div class="bar-track">
                <div
                  class="bar-fill"
                  style="width: {visible ? skill.level : 0}%; background: {current.color}; transition-delay: {i * 0.06 + 0.3}s"
                ></div>
              </div>
            </div>
          {/each}
        </div>

        <!-- Proficiency legend -->
        <div class="legend">
          <span class="legend-item"><span class="l-dot" style="background: var(--accent2)"></span>Expert (90%+)</span>
          <span class="legend-item"><span class="l-dot" style="background: var(--accent)"></span>Proficient (75–89%)</span>
          <span class="legend-item"><span class="l-dot" style="background: var(--accent3)"></span>Intermediate (60–74%)</span>
        </div>
      </div>
    </div>

    <!-- Tools & tech cloud -->
    <div class="tools-section">
      <p class="tools-label">
        <span class="mono-prefix">// </span>Tools & Technologies
      </p>
      <div class="tools-grid">
        {#each tools as tool, i}
          <span class="tool-chip" style="animation-delay: {i * 0.04}s">{tool}</span>
        {/each}
      </div>
    </div>
  </div>
</section>

<style>
  section {
    padding: 7rem 2rem;
    opacity: 0;
    transform: translateY(20px);
    transition: opacity 0.6s ease, transform 0.6s ease;
  }

  section.visible { opacity: 1; transform: translateY(0); }

  .container { max-width: 1100px; margin: 0 auto; }

  .accent { color: var(--accent); }

  .content {
    margin-top: 3rem;
    display: grid;
    grid-template-columns: 280px 1fr;
    gap: 2.5rem;
    align-items: start;
  }

  /* Category tabs */
  .cat-tabs {
    display: flex;
    flex-direction: column;
    gap: 0.5rem;
  }

  .cat-tabs button {
    display: flex;
    align-items: center;
    gap: 0.75rem;
    background: var(--surface);
    border: 1px solid var(--border);
    color: var(--text-dim);
    font-family: var(--mono);
    font-size: 0.78rem;
    padding: 1rem 1.25rem;
    border-radius: 8px;
    cursor: none;
    text-align: left;
    transition: all 0.2s;
    letter-spacing: 0.02em;
  }

  .cat-tabs button:hover {
    border-color: rgba(255,255,255,0.12);
    color: var(--text);
  }

  .cat-tabs button.active {
    border-color: var(--cat-color);
    color: var(--cat-color);
    background: rgba(255,255,255,0.03);
    box-shadow: 0 0 20px color-mix(in srgb, var(--cat-color) 15%, transparent);
  }

  .cat-icon {
    font-size: 1rem;
    font-style: normal;
  }

  /* Skills panel */
  .skills-panel {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 10px;
    padding: 2rem;
  }

  .skill-list {
    display: flex;
    flex-direction: column;
    gap: 1.2rem;
    margin-bottom: 1.5rem;
  }

  .skill-row {
    opacity: 0;
    animation: slideIn 0.4s ease forwards;
  }

  section.visible .skill-row { opacity: 1; }

  .skill-head {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 0.4rem;
  }

  .skill-name {
    font-family: var(--mono);
    font-size: 0.78rem;
    color: var(--text-bright);
    display: flex;
    align-items: center;
    gap: 0.5rem;
    letter-spacing: 0.02em;
  }

  .skill-icon { font-size: 0.9rem; }

  .skill-pct {
    font-family: var(--display);
    font-size: 0.75rem;
    font-weight: 700;
    letter-spacing: 0.05em;
  }

  .bar-track {
    height: 4px;
    background: rgba(255,255,255,0.06);
    border-radius: 100px;
    overflow: hidden;
  }

  .bar-fill {
    height: 100%;
    border-radius: 100px;
    width: 0;
    transition: width 0.9s cubic-bezier(0.4, 0, 0.2, 1);
    position: relative;
  }

  .bar-fill::after {
    content: '';
    position: absolute;
    right: 0;
    top: 50%;
    transform: translateY(-50%);
    width: 6px;
    height: 6px;
    border-radius: 50%;
    background: inherit;
    box-shadow: 0 0 6px currentColor;
  }

  /* Legend */
  .legend {
    display: flex;
    gap: 1.5rem;
    flex-wrap: wrap;
    padding-top: 1rem;
    border-top: 1px solid var(--border);
  }

  .legend-item {
    display: flex;
    align-items: center;
    gap: 6px;
    font-family: var(--mono);
    font-size: 0.62rem;
    color: var(--text-dim);
    letter-spacing: 0.04em;
  }

  .l-dot {
    width: 7px; height: 7px;
    border-radius: 50%;
  }

  /* Tools section */
  .tools-section {
    margin-top: 4rem;
  }

  .tools-label {
    font-family: var(--mono);
    font-size: 0.75rem;
    color: var(--text-dim);
    margin-bottom: 1.2rem;
    letter-spacing: 0.08em;
  }

  .mono-prefix { color: var(--accent2); }

  .tools-grid {
    display: flex;
    flex-wrap: wrap;
    gap: 0.6rem;
  }

  .tool-chip {
    font-family: var(--mono);
    font-size: 0.72rem;
    color: var(--text);
    background: var(--surface);
    border: 1px solid var(--border);
    padding: 0.4rem 0.9rem;
    border-radius: 4px;
    letter-spacing: 0.04em;
    transition: all 0.2s;
    opacity: 0;
    animation: chipIn 0.3s ease forwards;
    cursor: default;
  }

  section.visible .tool-chip { opacity: 1; }

  .tool-chip:hover {
    border-color: var(--border-bright);
    color: var(--accent);
    background: rgba(0,200,255,0.05);
  }

  @keyframes slideIn {
    from { opacity: 0; transform: translateX(-10px); }
    to { opacity: 1; transform: translateX(0); }
  }

  @keyframes chipIn {
    from { opacity: 0; transform: scale(0.9); }
    to { opacity: 1; transform: scale(1); }
  }

  @media (max-width: 768px) {
    .content { grid-template-columns: 1fr; }
    .cat-tabs { flex-direction: row; overflow-x: auto; }
    .cat-tabs button { flex-shrink: 0; padding: 0.7rem 1rem; }
    section { padding: 5rem 1.5rem; }
  }
</style>
