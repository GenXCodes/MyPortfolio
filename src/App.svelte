<script>
  import Nav from './lib/components/Nav.svelte'
  import Hero from './lib/components/Hero.svelte'
  import About from './lib/components/About.svelte'
  import Projects from './lib/components/Projects.svelte'
  import Skills from './lib/components/Skills.svelte'
  import Contact from './lib/components/Contact.svelte'
  import Cursor from './lib/components/Cursor.svelte'
  import Footer from './lib/components/Footer.svelte'
  import BackToTop from './lib/components/BackToTop.svelte'
</script>

<Cursor />
<Nav />

<main>
  <Hero />
  <About />
  <Projects />
  <Skills />
  <Contact />
</main>

<Footer />
<BackToTop />

<style>
  main {
    position: relative;
    z-index: 1;
  }
</style>
