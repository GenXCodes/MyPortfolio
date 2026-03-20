<script>
  import { onMount } from 'svelte'

  let visible = $state(false)
  let sectionEl = $state(null)

  let form = $state({ name: '', email: '', subject: '', message: '' })
  let status = $state('idle') // idle | sending | sent | error
  let errors = $state({})

  onMount(() => {
    const observer = new IntersectionObserver(
      ([entry]) => { if (entry.isIntersecting) visible = true },
      { threshold: 0.1 }
    )
    if (sectionEl) observer.observe(sectionEl)
    return () => observer.disconnect()
  })

  function validate() {
    const e = {}
    if (!form.name.trim()) e.name = 'Name is required'
    if (!form.email.match(/^[^\s@]+@[^\s@]+\.[^\s@]+$/)) e.email = 'Valid email required'
    if (!form.subject.trim()) e.subject = 'Subject is required'
    if (form.message.trim().length < 10) e.message = 'Message too short (min 10 chars)'
    errors = e
    return Object.keys(e).length === 0
  }

  async function handleSubmit() {
    if (!validate()) return
    status = 'sending'

    // Replace with your actual form endpoint (e.g. Formspree, EmailJS, etc.)
    // await fetch('https://formspree.io/f/YOUR_ID', { method: 'POST', body: JSON.stringify(form), headers: { 'Content-Type': 'application/json' } })

    // Simulated delay for demo
    await new Promise(r => setTimeout(r, 1500))
    status = 'sent'
    form = { name: '', email: '', subject: '', message: '' }
  }

  const contacts = [
    {
      icon: `<svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5"><path d="M4 4h16c1.1 0 2 .9 2 2v12c0 1.1-.9 2-2 2H4c-1.1 0-2-.9-2-2V6c0-1.1.9-2 2-2z"/><polyline points="22,6 12,13 2,6"/></svg>`,
      label: 'Email',
      value: 'your@email.com',
      link: 'mailto:your@email.com',
    },
    {
      icon: `<svg width="18" height="18" viewBox="0 0 24 24" fill="currentColor"><path d="M20.317 4.37a19.791 19.791 0 0 0-4.885-1.515.074.074 0 0 0-.079.037c-.21.375-.444.864-.608 1.25a18.27 18.27 0 0 0-5.487 0 12.64 12.64 0 0 0-.617-1.25.077.077 0 0 0-.079-.037A19.736 19.736 0 0 0 3.677 4.37a.07.07 0 0 0-.032.027C.533 9.046-.32 13.58.099 18.057.1 18.079.11 18.1.12 18.12a19.9 19.9 0 0 0 5.993 3.03.078.078 0 0 0 .084-.028c.462-.63.874-1.295 1.226-1.994a.076.076 0 0 0-.041-.106 13.107 13.107 0 0 1-1.872-.892.077.077 0 0 1-.008-.128 10.2 10.2 0 0 0 .372-.292.074.074 0 0 1 .077-.01c3.928 1.793 8.18 1.793 12.062 0a.074.074 0 0 1 .078.01c.12.098.246.198.373.292a.077.077 0 0 1-.006.127 12.299 12.299 0 0 1-1.873.892.077.077 0 0 0-.041.107c.36.698.772 1.362 1.225 1.993a.076.076 0 0 0 .084.028 19.839 19.839 0 0 0 6.002-3.03.077.077 0 0 0 .032-.054c.5-5.177-.838-9.674-3.549-13.66a.061.061 0 0 0-.031-.03z"/></svg>`,
      label: 'Discord',
      value: 'yourhandle#0000',
      link: 'https://discord.com',
    },
    {
      icon: `<svg width="18" height="18" viewBox="0 0 24 24" fill="currentColor"><path d="M20.447 20.452h-3.554v-5.569c0-1.328-.027-3.037-1.852-3.037-1.853 0-2.136 1.445-2.136 2.939v5.667H9.351V9h3.414v1.561h.046c.477-.9 1.637-1.85 3.37-1.85 3.601 0 4.267 2.37 4.267 5.455v6.286zM5.337 7.433c-1.144 0-2.063-.926-2.063-2.065 0-1.138.92-2.063 2.063-2.063 1.14 0 2.064.925 2.064 2.063 0 1.139-.925 2.065-2.064 2.065zm1.782 13.019H3.555V9h3.564v11.452zM22.225 0H1.771C.792 0 0 .774 0 1.729v20.542C0 23.227.792 24 1.771 24h20.451C23.2 24 24 23.227 24 22.271V1.729C24 .774 23.2 0 22.222 0h.003z"/></svg>`,
      label: 'LinkedIn',
      value: 'linkedin.com/in/yourprofile',
      link: 'https://linkedin.com',
    },
    {
      icon: `<svg width="18" height="18" viewBox="0 0 24 24" fill="currentColor"><path d="M12 0C5.37 0 0 5.37 0 12c0 5.31 3.435 9.795 8.205 11.385.6.105.825-.255.825-.57 0-.285-.015-1.23-.015-2.235-3.015.555-3.795-.735-4.035-1.41-.135-.345-.72-1.41-1.23-1.695-.42-.225-1.02-.78-.015-.795.945-.015 1.62.87 1.845 1.23 1.08 1.815 2.805 1.305 3.495.99.105-.78.42-1.305.765-1.605-2.67-.3-5.46-1.335-5.46-5.925 0-1.305.465-2.385 1.23-3.225-.12-.3-.54-1.53.12-3.18 0 0 1.005-.315 3.3 1.23.96-.27 1.98-.405 3-.405s2.04.135 3 .405c2.295-1.56 3.3-1.23 3.3-1.23.66 1.65.24 2.88.12 3.18.765.84 1.23 1.905 1.23 3.225 0 4.605-2.805 5.625-5.475 5.925.435.375.81 1.095.81 2.22 0 1.605-.015 2.895-.015 3.3 0 .315.225.69.825.57A12.02 12.02 0 0 0 24 12c0-6.63-5.37-12-12-12z"/></svg>`,
      label: 'GitHub',
      value: 'github.com/yourusername',
      link: 'https://github.com',
    },
  ]
</script>

<section id="contact" bind:this={sectionEl} class:visible>
  <div class="container">
    <div class="header">
      <p class="section-label">04 / contact</p>
      <h2 class="section-title">Get In Touch<span class="accent">_</span></h2>
      <p class="subtitle">
        Have a project in mind? Need IT support or an AI automation solution?
        Let's talk.
      </p>
    </div>

    <div class="content">
      <!-- Left: contact info -->
      <div class="left">
        <p class="left-intro">
          I'm currently <span class="highlight">open to new opportunities</span> —
          whether that's a full-time role, freelance project, or a quick consultation.
          Don't hesitate to reach out.
        </p>

        <div class="contact-list">
          {#each contacts as c, i}
            <a href={c.link} class="contact-item" target="_blank" rel="noopener" style="animation-delay: {i * 0.08 + 0.2}s">
              <span class="contact-icon">{@html c.icon}</span>
              <div>
                <span class="contact-label">{c.label}</span>
                <span class="contact-value">{c.value}</span>
              </div>
              <span class="arrow">→</span>
            </a>
          {/each}
        </div>

        <!-- Availability -->
        <div class="availability">
          <div class="avail-header">
            <span class="avail-dot"></span>
            <span>Current Availability</span>
          </div>
          <div class="avail-grid">
            <div class="avail-item open">
              <span>Freelance Projects</span>
              <span class="avail-status">Open</span>
            </div>
            <div class="avail-item open">
              <span>Full-time Roles</span>
              <span class="avail-status">Open</span>
            </div>
            <div class="avail-item limited">
              <span>Consultation</span>
              <span class="avail-status">Limited</span>
            </div>
          </div>
        </div>
      </div>

      <!-- Right: contact form -->
      <div class="right">
        {#if status === 'sent'}
          <div class="success-state">
            <div class="success-icon">✓</div>
            <h3>Message Received!</h3>
            <p>Thanks for reaching out. I'll get back to you within 24 hours.</p>
            <button class="reset-btn" onclick={() => status = 'idle'}>Send Another</button>
          </div>
        {:else}
          <form onsubmit={(e) => { e.preventDefault(); handleSubmit() }}>
            <div class="form-row">
              <div class="field">
                <label for="name">
                  <span class="field-prefix">// </span>Name
                </label>
                <input
                  id="name"
                  type="text"
                  placeholder="Your Name"
                  bind:value={form.name}
                  class:error={errors.name}
                />
                {#if errors.name}<span class="err">{errors.name}</span>{/if}
              </div>
              <div class="field">
                <label for="email">
                  <span class="field-prefix">// </span>Email
                </label>
                <input
                  id="email"
                  type="email"
                  placeholder="your@email.com"
                  bind:value={form.email}
                  class:error={errors.email}
                />
                {#if errors.email}<span class="err">{errors.email}</span>{/if}
              </div>
            </div>

            <div class="field">
              <label for="subject">
                <span class="field-prefix">// </span>Subject
              </label>
              <input
                id="subject"
                type="text"
                placeholder="What's this about?"
                bind:value={form.subject}
                class:error={errors.subject}
              />
              {#if errors.subject}<span class="err">{errors.subject}</span>{/if}
            </div>

            <div class="field">
              <label for="message">
                <span class="field-prefix">// </span>Message
              </label>
              <textarea
                id="message"
                rows="6"
                placeholder="Tell me about your project or inquiry..."
                bind:value={form.message}
                class:error={errors.message}
              ></textarea>
              {#if errors.message}<span class="err">{errors.message}</span>{/if}
            </div>

            <button type="submit" class="submit-btn" disabled={status === 'sending'}>
              {#if status === 'sending'}
                <span class="spinner"></span> Sending...
              {:else}
                <span>Send Message</span>
                <span class="send-arrow">→</span>
              {/if}
            </button>
          </form>
        {/if}
      </div>
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

  .header { margin-bottom: 4rem; }

  .subtitle {
    font-family: var(--body);
    font-size: 1rem;
    color: var(--text-dim);
    margin-top: 0.75rem;
    max-width: 500px;
    line-height: 1.6;
    font-weight: 300;
  }

  .accent { color: var(--accent); }

  .content {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 4rem;
  }

  /* Left */
  .left-intro {
    font-family: var(--body);
    font-size: 0.95rem;
    line-height: 1.75;
    color: var(--text);
    margin-bottom: 2rem;
    font-weight: 300;
  }

  .highlight { color: var(--accent2); font-weight: 500; }

  .contact-list {
    display: flex;
    flex-direction: column;
    gap: 0.6rem;
    margin-bottom: 2rem;
  }

  .contact-item {
    display: flex;
    align-items: center;
    gap: 1rem;
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 8px;
    padding: 0.9rem 1rem;
    transition: all 0.2s;
    opacity: 0;
    animation: fadeUp 0.4s ease forwards;
    cursor: pointer;
  }

  section.visible .contact-item { opacity: 1; }

  .contact-item:hover {
    border-color: var(--border-bright);
    box-shadow: 0 4px 20px rgba(0,0,0,0.2);
    transform: translateX(4px);
  }

  .contact-icon {
    color: var(--accent);
    display: flex;
    align-items: center;
    flex-shrink: 0;
    width: 36px;
    height: 36px;
    background: rgba(0,200,255,0.08);
    border-radius: 6px;
    justify-content: center;
  }

  .contact-label {
    display: block;
    font-family: var(--mono);
    font-size: 0.6rem;
    color: var(--text-dim);
    text-transform: uppercase;
    letter-spacing: 0.1em;
    margin-bottom: 2px;
  }

  .contact-value {
    display: block;
    font-family: var(--body);
    font-size: 0.85rem;
    color: var(--text-bright);
  }

  .arrow {
    margin-left: auto;
    color: var(--text-dim);
    font-size: 1rem;
    transition: transform 0.2s, color 0.2s;
  }

  .contact-item:hover .arrow { transform: translateX(4px); color: var(--accent); }

  /* Availability */
  .availability {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 8px;
    padding: 1.25rem;
  }

  .avail-header {
    display: flex;
    align-items: center;
    gap: 0.5rem;
    font-family: var(--mono);
    font-size: 0.7rem;
    color: var(--text-dim);
    text-transform: uppercase;
    letter-spacing: 0.1em;
    margin-bottom: 1rem;
  }

  .avail-dot {
    width: 7px; height: 7px;
    border-radius: 50%;
    background: var(--accent2);
    animation: pulse 2s ease infinite;
  }

  .avail-grid {
    display: flex;
    flex-direction: column;
    gap: 0.5rem;
  }

  .avail-item {
    display: flex;
    justify-content: space-between;
    align-items: center;
    font-family: var(--mono);
    font-size: 0.75rem;
    color: var(--text);
    padding: 0.4rem 0;
    border-bottom: 1px solid var(--border);
  }

  .avail-item:last-child { border-bottom: none; }

  .avail-status {
    font-size: 0.65rem;
    padding: 2px 8px;
    border-radius: 100px;
  }

  .avail-item.open .avail-status {
    background: rgba(0,255,157,0.1);
    border: 1px solid rgba(0,255,157,0.3);
    color: var(--accent2);
  }

  .avail-item.limited .avail-status {
    background: rgba(255,111,60,0.1);
    border: 1px solid rgba(255,111,60,0.3);
    color: var(--accent3);
  }

  /* Form */
  form {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 10px;
    padding: 2rem;
    display: flex;
    flex-direction: column;
    gap: 1.25rem;
  }

  .form-row { display: grid; grid-template-columns: 1fr 1fr; gap: 1rem; }

  .field { display: flex; flex-direction: column; gap: 0.4rem; }

  label {
    font-family: var(--mono);
    font-size: 0.68rem;
    color: var(--text-dim);
    letter-spacing: 0.08em;
    text-transform: uppercase;
  }

  .field-prefix { color: var(--accent2); }

  input, textarea {
    background: rgba(255,255,255,0.03);
    border: 1px solid var(--border);
    border-radius: 6px;
    padding: 0.75rem 1rem;
    color: var(--text-bright);
    font-family: var(--mono);
    font-size: 0.82rem;
    outline: none;
    transition: border-color 0.2s, box-shadow 0.2s;
    resize: vertical;
    cursor: text;
  }

  input::placeholder, textarea::placeholder { color: var(--text-dim); }

  input:focus, textarea:focus {
    border-color: var(--accent);
    box-shadow: 0 0 0 2px rgba(0,200,255,0.1);
  }

  input.error, textarea.error { border-color: #ff4444; }

  .err {
    font-family: var(--mono);
    font-size: 0.62rem;
    color: #ff6666;
    letter-spacing: 0.04em;
  }

  .submit-btn {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 0.6rem;
    background: var(--accent);
    color: var(--bg);
    border: none;
    font-family: var(--mono);
    font-size: 0.82rem;
    font-weight: 700;
    padding: 1rem 2rem;
    border-radius: 6px;
    cursor: none;
    letter-spacing: 0.06em;
    transition: all 0.2s;
    box-shadow: 0 0 20px rgba(0,200,255,0.25);
    position: relative;
    overflow: hidden;
  }

  .submit-btn:disabled { opacity: 0.7; cursor: not-allowed; }

  .submit-btn:not(:disabled):hover {
    background: #33d6ff;
    box-shadow: 0 0 30px rgba(0,200,255,0.4);
    transform: translateY(-2px);
  }

  .send-arrow { transition: transform 0.2s; }
  .submit-btn:hover .send-arrow { transform: translateX(4px); }

  .spinner {
    width: 14px; height: 14px;
    border: 2px solid rgba(0,0,0,0.3);
    border-top-color: var(--bg);
    border-radius: 50%;
    animation: spin 0.7s linear infinite;
  }

  /* Success state */
  .success-state {
    background: var(--surface);
    border: 1px solid rgba(0,255,157,0.3);
    border-radius: 10px;
    padding: 3rem 2rem;
    text-align: center;
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 1rem;
  }

  .success-icon {
    width: 56px; height: 56px;
    border-radius: 50%;
    background: rgba(0,255,157,0.1);
    border: 2px solid var(--accent2);
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 1.5rem;
    color: var(--accent2);
  }

  .success-state h3 {
    font-family: var(--display);
    font-size: 1.3rem;
    color: var(--text-bright);
  }

  .success-state p {
    font-family: var(--body);
    color: var(--text);
    font-size: 0.9rem;
  }

  .reset-btn {
    background: transparent;
    border: 1px solid var(--border-bright);
    color: var(--text);
    font-family: var(--mono);
    font-size: 0.78rem;
    padding: 0.6rem 1.4rem;
    border-radius: 4px;
    cursor: none;
    transition: all 0.2s;
    margin-top: 0.5rem;
  }

  .reset-btn:hover { border-color: var(--accent); color: var(--accent); }

  @keyframes pulse {
    0%, 100% { opacity: 1; transform: scale(1); }
    50% { opacity: 0.5; transform: scale(1.3); }
  }

  @keyframes spin {
    to { transform: rotate(360deg); }
  }

  @keyframes fadeUp {
    from { opacity: 0; transform: translateY(10px); }
    to { opacity: 1; transform: translateY(0); }
  }

  @media (max-width: 768px) {
    .content { grid-template-columns: 1fr; gap: 2.5rem; }
    .form-row { grid-template-columns: 1fr; }
    section { padding: 5rem 1.5rem; }
  }
</style>
