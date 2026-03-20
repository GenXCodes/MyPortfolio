<script>
  import { onMount } from 'svelte'

  let visible = $state(false)
  let sectionEl = $state(null)

  onMount(() => {
    const observer = new IntersectionObserver(
      ([entry]) => { if (entry.isIntersecting) visible = true },
      { threshold: 0.2 }
    )
    if (sectionEl) observer.observe(sectionEl)
    return () => observer.disconnect()
  })

  const facts = [
    { icon: '🌏', label: 'Based in', value: 'Philippines' },
    { icon: '💼', label: 'Available for', value: 'Freelance & Full-time' },
    { icon: '⚡', label: 'Focus area', value: 'Web Dev & IT Support' },
    { icon: '🤖', label: 'Specialty', value: 'AI Automations & MSP' },
  ]
</script>

<section id="about" bind:this={sectionEl} class:visible>
  <div class="container">
    <div class="left">
      <p class="section-label">01 / about</p>
      <h2 class="section-title">Who Am I<span class="accent">_</span></h2>

      <div class="bio">
        <p>
          Hey! I'm <strong>[Your Name]</strong>, a tech professional passionate about
          building reliable digital systems. I bridge the gap between end-users and
          complex technology — whether that's developing web applications, managing
          MSP support environments, or crafting intelligent AI automation workflows.
        </p>
        <p>
          With hands-on experience in IT infrastructure, modern web frameworks, and
          AI tooling, I approach every problem with both technical precision and a
          user-first mindset.
        </p>
        <p>
          When I'm not coding or troubleshooting, I'm exploring the latest in AI
          tooling and automation to keep my skills on the cutting edge.
        </p>
      </div>

      <div class="btn-wrapper">
        <a href="/resume.pdf" download class="resume-btn">
          <span class="icon">↓</span>
          Download Resume
        </a>
      </div>
    </div>

    <div class="right">
      <!-- Avatar / Profile card -->
      <div class="profile-card">
        <div class="avatar-wrapper">
          <div class="avatar-ring"></div>
          <div class="avatar-ring ring2"></div>
          <div class="avatar">
            <span class="avatar-initials">YN</span>
          </div>
          <!-- Replace the above div.avatar block with: -->
          <!-- <img src="/your-photo.jpg" alt="Your Name" class="avatar-img" /> -->
        </div>

        <div class="card-info">
          <p class="card-name">[Your Name]</p>
          <p class="card-role">Web Dev & Tech Support</p>
          <div class="status-badge">
            <span class="status-dot"></span>
            Available for work
          </div>
        </div>
      </div>

      <!-- Fact grid -->
      <div class="fact-grid">
        {#each facts as fact, i}
          <div class="fact" style="animation-delay:{i * 0.1 + 0.4}s">
            <span class="fact-icon">{fact.icon}</span>
            <div>
              <span class="fact-label">{fact.label}</span>
              <span class="fact-value">{fact.value}</span>
            </div>
          </div>
        {/each}
      </div>

      <!-- Code snippet decoration -->
      <div class="code-deco" aria-hidden="true">
        <span class="c-keyword">const</span>
        <span class="c-var"> me</span>
        <span class="c-op"> = </span>
        <span class="c-bracket">&#123;</span><br>
        &nbsp;&nbsp;<span class="c-key">role:</span>
        <span class="c-str"> "Tech Pro"</span><span class="c-op">,</span><br>
        &nbsp;&nbsp;<span class="c-key">passion:</span>
        <span class="c-str"> "Building things"</span><span class="c-op">,</span><br>
        &nbsp;&nbsp;<span class="c-key">coffee:</span>
        <span class="c-bool"> true</span><br>
        <span class="c-bracket">&#125;</span>
      </div>
    </div>
  </div>
</section>

<style>
  section {
    padding: 7rem 2rem;
    opacity: 0;
    transform: translateY(30px);
    transition: opacity 0.7s ease, transform 0.7s ease;
  }

  section.visible {
    opacity: 1;
    transform: translateY(0);
  }

  .container {
    max-width: 1100px;
    margin: 0 auto;
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 5rem;
    align-items: start;
  }

  /* Left */
  .bio {
    margin: 2rem 0;
    display: flex;
    flex-direction: column;
    gap: 1rem;
  }

  .bio p {
    font-family: var(--body);
    font-size: 1rem;
    line-height: 1.8;
    color: var(--text);
    font-weight: 300;
  }

  .bio strong {
    color: var(--accent);
    font-weight: 600;
  }

  .resume-btn {
    display: inline-flex;
    align-items: center;
    gap: 0.6rem;
    border: 1px solid var(--border-bright);
    color: var(--text-bright);
    font-family: var(--mono);
    font-size: 0.8rem;
    padding: 0.75rem 1.5rem;
    border-radius: 4px;
    cursor: pointer;
    transition: all 0.2s;
    letter-spacing: 0.05em;
  }

  .resume-btn:hover {
    border-color: var(--accent2);
    color: var(--accent2);
    box-shadow: 0 0 15px rgba(0,255,157,0.15);
  }

  .icon { font-style: normal; }
  .accent { color: var(--accent); }

  /* Right */
  .right {
    display: flex;
    flex-direction: column;
    gap: 1.5rem;
  }

  .profile-card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 12px;
    padding: 1.5rem;
    display: flex;
    align-items: center;
    gap: 1.5rem;
    position: relative;
    overflow: hidden;
  }

  .profile-card::before {
    content: '';
    position: absolute;
    top: 0; left: 0; right: 0;
    height: 2px;
    background: linear-gradient(90deg, var(--accent), var(--accent2));
  }

  .avatar-wrapper {
    position: relative;
    width: 72px;
    height: 72px;
    flex-shrink: 0;
  }

  .avatar-ring {
    position: absolute;
    inset: -6px;
    border-radius: 50%;
    border: 1px solid rgba(0,200,255,0.3);
    animation: spin 8s linear infinite;
  }

  .ring2 {
    inset: -12px;
    border-color: rgba(0,255,157,0.15);
    animation-duration: 12s;
    animation-direction: reverse;
  }

  .avatar {
    width: 72px;
    height: 72px;
    border-radius: 50%;
    background: linear-gradient(135deg, var(--surface2), var(--bg2));
    border: 2px solid var(--accent);
    display: flex;
    align-items: center;
    justify-content: center;
    position: relative;
    z-index: 1;
  }

  .avatar-initials {
    font-family: var(--display);
    font-size: 1.2rem;
    font-weight: 700;
    color: var(--accent);
  }

  /* Swap .avatar div for <img class="avatar-img"> if you have a photo:
     width: 72px; height: 72px; border-radius: 50%; object-fit: cover;
     border: 2px solid var(--accent); position: relative; z-index: 1; */

  .card-name {
    font-family: var(--display);
    font-size: 1.1rem;
    color: var(--text-bright);
    font-weight: 700;
    margin-bottom: 2px;
  }

  .card-role {
    font-family: var(--mono);
    font-size: 0.72rem;
    color: var(--text-dim);
    margin-bottom: 0.7rem;
    letter-spacing: 0.05em;
  }

  .status-badge {
    display: inline-flex;
    align-items: center;
    gap: 6px;
    background: rgba(0,255,157,0.08);
    border: 1px solid rgba(0,255,157,0.2);
    color: var(--accent2);
    font-family: var(--mono);
    font-size: 0.65rem;
    padding: 0.25rem 0.65rem;
    border-radius: 100px;
    letter-spacing: 0.05em;
  }

  .status-dot {
    width: 6px; height: 6px;
    border-radius: 50%;
    background: var(--accent2);
    animation: pulse 2s ease infinite;
  }

  /* Fact grid */
  .fact-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 0.75rem;
  }

  .fact {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 8px;
    padding: 0.9rem 1rem;
    display: flex;
    align-items: center;
    gap: 0.75rem;
    opacity: 0;
    animation: fadeUp 0.5s ease forwards;
    transition: border-color 0.2s, box-shadow 0.2s;
  }

  section.visible .fact { opacity: 1; }

  .fact:hover {
    border-color: var(--border-bright);
    box-shadow: 0 0 15px rgba(0,200,255,0.06);
  }

  .fact-icon { font-size: 1.2rem; }

  .fact-label {
    display: block;
    font-family: var(--mono);
    font-size: 0.6rem;
    color: var(--text-dim);
    text-transform: uppercase;
    letter-spacing: 0.1em;
    margin-bottom: 2px;
  }

  .fact-value {
    display: block;
    font-family: var(--body);
    font-size: 0.82rem;
    color: var(--text-bright);
    font-weight: 500;
  }

  /* Code decoration */
  .code-deco {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 8px;
    padding: 1rem 1.25rem;
    font-family: var(--mono);
    font-size: 0.75rem;
    line-height: 1.8;
  }

  .c-keyword { color: #ff79c6; }
  .c-var { color: var(--text-bright); }
  .c-op { color: var(--text-dim); }
  .c-bracket { color: var(--accent2); }
  .c-key { color: var(--accent); }
  .c-str { color: #f1fa8c; }
  .c-bool { color: #bd93f9; }

  /* Animations */
  @keyframes spin {
    from { transform: rotate(0deg); }
    to { transform: rotate(360deg); }
  }

  @keyframes pulse {
    0%, 100% { opacity: 1; transform: scale(1); }
    50% { opacity: 0.5; transform: scale(1.3); }
  }

  @keyframes fadeUp {
    from { opacity: 0; transform: translateY(12px); }
    to { opacity: 1; transform: translateY(0); }
  }

  @media (max-width: 768px) {
    .container { grid-template-columns: 1fr; gap: 3rem; }
    section { padding: 5rem 1.5rem; }
  }
</style>
