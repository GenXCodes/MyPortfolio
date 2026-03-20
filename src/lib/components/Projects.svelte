<script>
  import { onMount } from 'svelte'

  let visible = $state(false)
  let sectionEl = $state(null)
  let activeFilter = $state('all')

  onMount(() => {
    const observer = new IntersectionObserver(
      ([entry]) => { if (entry.isIntersecting) visible = true },
      { threshold: 0.1 }
    )
    if (sectionEl) observer.observe(sectionEl)
    return () => observer.disconnect()
  })

  const filters = ['all', 'web', 'automation', 'support']

  const projects = [
    {
      id: 1,
      title: 'IT Helpdesk Portal',
      desc: 'Full-featured MSP ticketing system with SLA tracking, auto-escalation, and client portal. Reduced average resolution time by 40%.',
      tags: ['SvelteKit', 'Supabase', 'PostgreSQL', 'Tailwind'],
      category: 'support',
      status: 'live',
      link: '#',
      github: '#',
      accent: '#00c8ff',
    },
    {
      id: 2,
      title: 'AI Workflow Automator',
      desc: 'N8N + OpenAI powered automation suite for repetitive business tasks — email triage, data extraction, report generation.',
      tags: ['N8N', 'OpenAI API', 'Zapier', 'Python'],
      category: 'automation',
      status: 'live',
      link: '#',
      github: '#',
      accent: '#00ff9d',
    },
    {
      id: 3,
      title: 'Company Dashboard',
      desc: 'Internal operations dashboard with real-time metrics, team activity feed, and integrated Microsoft 365 data.',
      tags: ['React', 'TypeScript', 'Power BI', 'Azure AD'],
      category: 'web',
      status: 'live',
      link: '#',
      github: '#',
      accent: '#ff6f3c',
    },
    {
      id: 4,
      title: 'Network Monitor Bot',
      desc: 'Discord/Slack bot that monitors server uptime, sends alerts, and generates weekly health reports for managed clients.',
      tags: ['Node.js', 'Discord.js', 'SNMP', 'Grafana'],
      category: 'support',
      status: 'wip',
      link: '#',
      github: '#',
      accent: '#00c8ff',
    },
    {
      id: 5,
      title: 'Client Onboarding Site',
      desc: 'Automated client onboarding web app with document collection, e-signatures, and CRM integration.',
      tags: ['SvelteKit', 'Stripe', 'DocuSign API', 'Prisma'],
      category: 'web',
      status: 'live',
      link: '#',
      github: '#',
      accent: '#00ff9d',
    },
    {
      id: 6,
      title: 'AI Chat Support Agent',
      desc: 'Custom GPT-4o powered support agent trained on company KB articles. Handles 60% of L1 tickets autonomously.',
      tags: ['OpenAI', 'LangChain', 'Pinecone', 'FastAPI'],
      category: 'automation',
      status: 'live',
      link: '#',
      github: '#',
      accent: '#ff6f3c',
    },
  ]

  const filtered = $derived(
    activeFilter === 'all' ? projects : projects.filter(p => p.category === activeFilter)
  )
</script>

<section id="projects" bind:this={sectionEl} class:visible>
  <div class="container">
    <div class="header">
      <div>
        <p class="section-label">02 / projects</p>
        <h2 class="section-title">What I've Built<span class="accent">_</span></h2>
      </div>
      <p class="subtitle">
        A selection of real-world projects across web development,
        IT support, and AI automation.
      </p>
    </div>

    <!-- Filter tabs -->
    <div class="filters" role="tablist">
      {#each filters as f}
        <button
          role="tab"
          class:active={activeFilter === f}
          onclick={() => activeFilter = f}
        >
          {f === 'all' ? 'All Projects' : f.charAt(0).toUpperCase() + f.slice(1)}
          <span class="count">
            {f === 'all' ? projects.length : projects.filter(p => p.category === f).length}
          </span>
        </button>
      {/each}
    </div>

    <!-- Project grid -->
    <div class="grid">
      {#each filtered as project, i (project.id)}
        <article class="card" style="--card-accent: {project.accent}; animation-delay: {i * 0.08}s">
          <div class="card-top">
            <div class="card-meta">
              <span class="cat-tag">{project.category}</span>
              <span class="status" class:wip={project.status === 'wip'}>
                <span class="status-dot"></span>
                {project.status === 'wip' ? 'In Progress' : 'Live'}
              </span>
            </div>
            <div class="card-links">
              <a href={project.github} aria-label="GitHub" title="View code">
                <svg width="16" height="16" viewBox="0 0 24 24" fill="currentColor">
                  <path d="M12 0C5.37 0 0 5.37 0 12c0 5.31 3.435 9.795 8.205 11.385.6.105.825-.255.825-.57 0-.285-.015-1.23-.015-2.235-3.015.555-3.795-.735-4.035-1.41-.135-.345-.72-1.41-1.23-1.695-.42-.225-1.02-.78-.015-.795.945-.015 1.62.87 1.845 1.23 1.08 1.815 2.805 1.305 3.495.99.105-.78.42-1.305.765-1.605-2.67-.3-5.46-1.335-5.46-5.925 0-1.305.465-2.385 1.23-3.225-.12-.3-.54-1.53.12-3.18 0 0 1.005-.315 3.3 1.23.96-.27 1.98-.405 3-.405s2.04.135 3 .405c2.295-1.56 3.3-1.23 3.3-1.23.66 1.65.24 2.88.12 3.18.765.84 1.23 1.905 1.23 3.225 0 4.605-2.805 5.625-5.475 5.925.435.375.81 1.095.81 2.22 0 1.605-.015 2.895-.015 3.3 0 .315.225.69.825.57A12.02 12.02 0 0 0 24 12c0-6.63-5.37-12-12-12z"/>
                </svg>
              </a>
              <a href={project.link} aria-label="Live demo" title="View live">
                <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                  <path d="M18 13v6a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V8a2 2 0 0 1 2-2h6"/>
                  <polyline points="15 3 21 3 21 9"/>
                  <line x1="10" y1="14" x2="21" y2="3"/>
                </svg>
              </a>
            </div>
          </div>

          <h3 class="card-title">{project.title}</h3>
          <p class="card-desc">{project.desc}</p>

          <div class="card-tags">
            {#each project.tags as tag}
              <span class="tag">{tag}</span>
            {/each}
          </div>
        </article>
      {/each}
    </div>
  </div>
</section>

<style>
  section {
    padding: 7rem 2rem;
    background: var(--bg2);
    opacity: 0;
    transform: translateY(20px);
    transition: opacity 0.6s ease, transform 0.6s ease;
  }

  section.visible { opacity: 1; transform: translateY(0); }

  .container { max-width: 1100px; margin: 0 auto; }

  .header {
    display: flex;
    justify-content: space-between;
    align-items: flex-end;
    gap: 2rem;
    margin-bottom: 2.5rem;
    flex-wrap: wrap;
  }

  .subtitle {
    font-family: var(--body);
    font-size: 0.95rem;
    color: var(--text-dim);
    max-width: 300px;
    line-height: 1.6;
    font-weight: 300;
    text-align: right;
  }

  .accent { color: var(--accent); }

  /* Filters */
  .filters {
    display: flex;
    gap: 0.5rem;
    margin-bottom: 2.5rem;
    flex-wrap: wrap;
  }

  .filters button {
    display: flex;
    align-items: center;
    gap: 0.5rem;
    background: transparent;
    border: 1px solid var(--border);
    color: var(--text-dim);
    font-family: var(--mono);
    font-size: 0.75rem;
    padding: 0.5rem 1rem;
    border-radius: 4px;
    cursor: none;
    transition: all 0.2s;
    letter-spacing: 0.05em;
  }

  .filters button:hover {
    border-color: var(--border-bright);
    color: var(--text);
  }

  .filters button.active {
    border-color: var(--accent);
    color: var(--accent);
    background: rgba(0,200,255,0.08);
  }

  .count {
    background: rgba(255,255,255,0.08);
    border-radius: 100px;
    padding: 0 5px;
    font-size: 0.65rem;
  }

  .filters button.active .count {
    background: rgba(0,200,255,0.15);
  }

  /* Grid */
  .grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 1.25rem;
  }

  .card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 10px;
    padding: 1.5rem;
    position: relative;
    overflow: hidden;
    opacity: 0;
    animation: cardIn 0.4s ease forwards;
    transition: border-color 0.3s, box-shadow 0.3s, transform 0.3s;
    cursor: default;
  }

  section.visible .card { opacity: 1; }

  .card::before {
    content: '';
    position: absolute;
    top: 0; left: 0; right: 0;
    height: 2px;
    background: var(--card-accent);
    transform: scaleX(0);
    transform-origin: left;
    transition: transform 0.3s ease;
  }

  .card:hover {
    border-color: rgba(255,255,255,0.1);
    box-shadow: 0 8px 40px rgba(0,0,0,0.3), 0 0 0 1px rgba(255,255,255,0.04);
    transform: translateY(-4px);
  }

  .card:hover::before { transform: scaleX(1); }

  .card-top {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 1rem;
  }

  .card-meta { display: flex; align-items: center; gap: 0.6rem; }

  .cat-tag {
    font-family: var(--mono);
    font-size: 0.6rem;
    text-transform: uppercase;
    letter-spacing: 0.1em;
    color: var(--text-dim);
    background: var(--surface2);
    padding: 2px 8px;
    border-radius: 3px;
    border: 1px solid var(--border);
  }

  .status {
    font-family: var(--mono);
    font-size: 0.6rem;
    color: var(--accent2);
    display: flex;
    align-items: center;
    gap: 4px;
  }

  .status.wip { color: #febc2e; }

  .status-dot {
    width: 5px; height: 5px;
    border-radius: 50%;
    background: currentColor;
    animation: pulse 2s ease infinite;
  }

  .card-links {
    display: flex;
    gap: 0.6rem;
  }

  .card-links a {
    color: var(--text-dim);
    display: flex;
    align-items: center;
    transition: color 0.2s;
  }

  .card-links a:hover { color: var(--card-accent); }

  .card-title {
    font-family: var(--display);
    font-size: 1.05rem;
    font-weight: 700;
    color: var(--text-bright);
    margin-bottom: 0.65rem;
    letter-spacing: -0.01em;
  }

  .card-desc {
    font-family: var(--body);
    font-size: 0.85rem;
    line-height: 1.65;
    color: var(--text);
    margin-bottom: 1.2rem;
    font-weight: 300;
  }

  .card-tags {
    display: flex;
    flex-wrap: wrap;
    gap: 0.4rem;
  }

  .tag {
    font-family: var(--mono);
    font-size: 0.62rem;
    color: var(--text-dim);
    border: 1px solid var(--border);
    padding: 2px 8px;
    border-radius: 3px;
    transition: color 0.2s, border-color 0.2s;
  }

  .card:hover .tag {
    border-color: rgba(255,255,255,0.08);
    color: var(--text);
  }

  @keyframes cardIn {
    from { opacity: 0; transform: translateY(16px); }
    to { opacity: 1; transform: translateY(0); }
  }

  @keyframes pulse {
    0%, 100% { opacity: 1; }
    50% { opacity: 0.4; }
  }

  @media (max-width: 960px) {
    .grid { grid-template-columns: repeat(2, 1fr); }
  }

  @media (max-width: 600px) {
    .grid { grid-template-columns: 1fr; }
    section { padding: 5rem 1.5rem; }
    .subtitle { text-align: left; }
    .header { flex-direction: column; align-items: flex-start; }
  }
</style>
