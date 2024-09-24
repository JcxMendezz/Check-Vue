<template>
  <div id="app">
    <div class="stars"></div>
    <div class="twinkling"></div>
    <div class="content">
      <header>
        <h1>
          <span class="glitch" data-text="">HTTP-</span>
          <span class="glitch" data-text="">CAT-</span>
          <span class="glitch" data-text="">EXPLORER</span>
        </h1>
      </header>
      <main>
        <HttpCatViewer @status-change="updateBackground" />
      </main>
      <footer>
        <p>
          Creado con <span class="heart">❤️</span> y Vue.js | Imágenes de
          <a href="https://http.cat" target="_blank">http.cat</a>
        </p>
      </footer>
    </div>
  </div>
</template>

<script>
import HttpCatViewer from './components/HttpCatViewer.vue'

export default {
  name: 'App',
  components: {
    HttpCatViewer
  },
  methods: {
    updateBackground(status) {
      document.body.style.setProperty('--main-color', this.getColorByStatus(status))
    },
    getColorByStatus(status) {
      const statusGroup = Math.floor(status / 100)
      const colors = ['#2ecc71', '#3498db', '#f1c40f', '#e74c3c', '#9b59b6']
      return colors[statusGroup - 1] || '#34495e'
    }
  }
}
</script>

<style>
:root {
  --main-color: #3498db;
}

@keyframes move-twink-back {
  from {
    background-position: 0 0;
  }
  to {
    background-position: -10000px 5000px;
  }
}

@keyframes glitch {
  0% {
    text-shadow:
      0.05em 0 0 var(--main-color),
      -0.05em -0.025em 0 #0ff;
    clip-path: rect(1px, 9999px, 5px, 0);
  }
  25% {
    text-shadow:
      -0.05em -0.025em 0 var(--main-color),
      0.025em 0.025em 0 #0ff;
    clip-path: rect(5px, 9999px, 15px, 0);
  }
  50% {
    text-shadow:
      0.025em 0.05em 0 var(--main-color),
      -0.05em -0.05em 0 #0ff;
    clip-path: rect(10px, 9999px, 25px, 0);
  }
  75% {
    text-shadow:
      -0.05em -0.025em 0 var(--main-color),
      -0.025em -0.025em 0 #0ff;
    clip-path: rect(20px, 9999px, 35px, 0);
  }
  100% {
    text-shadow:
      -0.025em 0.025em 0 var(--main-color),
      0.05em 0 0 #0ff;
    clip-path: rect(30px, 9999px, 45px, 0);
  }
}

body,
html {
  margin: 0;
  padding: 0;
  block-size: 100%;
  font-family: 'Courier New', monospace;
  background-color: #000;
  color: #fff;
  overflow: hidden;
}

#app {
  block-size: 100vh;
  display: flex;
  flex-direction: column;
}

.stars,
.twinkling {
  position: absolute;
  inset-block-start: 0;
  inset-inline-start: 0;
  inset-inline-end: 0;
  inset-block-end: 0;
  inline-size: 100%;
  block-size: 100%;
  display: block;
}

.stars {
  background: #000 url(http://www.script-tutorials.com/demos/360/images/stars.png) repeat top center;
  z-index: 0;
}

.twinkling {
  background: transparent url(http://www.script-tutorials.com/demos/360/images/twinkling.png) repeat
    top center;
  z-index: 1;
  animation: move-twink-back 200s linear infinite;
}

.content {
  position: relative;
  z-index: 2;
  block-size: 100%;
  display: flex;
  flex-direction: column;
}

header {
  padding: 2rem;
  text-align: center;
}

h1 {
  font-size: 3rem;
  margin: 0;
}

.glitch {
  position: relative;
  color: #fff;
  animation: glitch 1s infinite linear alternate-reverse;
}

.glitch::before,
.glitch::after {
  content: attr(data-text);
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}

.glitch::before {
  left: 2px;
  text-shadow: -2px 0 #ff00c1;
  clip: rect(44px, 450px, 56px, 0);
  animation: glitch 5s infinite linear alternate-reverse;
}

.glitch::after {
  left: -2px;
  text-shadow:
    -2px 0 #00fff9,
    2px 2px #ff00c1;
  animation: glitch 1s infinite linear alternate-reverse;
}

main {
  flex-grow: 1;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 2rem;
}

footer {
  background-color: rgba(0, 0, 0, 0.5);
  text-align: center;
  padding: 1rem;
  font-size: 0.9rem;
}

footer a {
  color: var(--main-color);
  text-decoration: none;
}

footer a:hover {
  text-decoration: underline;
}

.heart {
  color: #e74c3c;
  animation: beat 0.25s infinite alternate;
  transform-origin: center;
}

@keyframes beat {
  to {
    transform: scale(1.4);
  }
}
</style>
