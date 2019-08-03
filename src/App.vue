<template>
  <div id="app">
    <h1>Scroll To Top Button</h1>
    <p v-for="i in 50" :key="i">Lorem ipsum dolor sit amet consectetur adipisicing elit. Repudiandae autem in fugiat placeat. Placeat temporibus expedita rerum omnis magni dolorem eum, obcaecati maxime blanditiis minus, tempora voluptatum unde tempore? Eveniet? Lorem ipsum dolor sit amet consectetur adipisicing elit. Ipsa aspernatur labore, hic nisi nemo eum maiores, atque officiis debitis saepe esse eos odit qui iste odio nesciunt vel, veritatis similique.</p>

    <transition name="slide-down-fade">
      <button
        v-if="scrollTopVisibile"
        class="app-scroll__btn"
        @click.prevent="scrollTop"
      >
        <span class="chevron top"></span>
      </button>
    </transition>
  </div>
</template>

<script>
const SCROLL_STATE = {
  NEUTRAL: 'neutral',
  UP: 'up',
  DOWN: 'down',
}

export default {
  name: 'app',

  data() {
    return {
      scroll: {
        current: SCROLL_STATE.NEUTRAL,
        buffer: 0,
      },
    }
  },

  computed: {
    scrollTopVisibile() {
      return this.scroll.current == SCROLL_STATE.UP && this.scroll.buffer != 0
    },
  },

  mounted() {
    window.addEventListener('scroll', this.handleWindowScroll, false)
  },

  beforeDestroy() {
    window.removeEventListener('scroll', this.handleWindowScroll)
  },

  methods: {
    handleWindowScroll() {
      var offset = window.pageYOffset ||
        document.documentElement.scrollTop
      if (offset > this.scroll.buffer) {
        this.scroll.current = SCROLL_STATE.DOWN
      } else if (offset < this.scroll.buffer) {
        this.scroll.current = SCROLL_STATE.UP
      }
      this.scroll.buffer = Math.max(0, offset)
    },

    scrollTop() {
      window.scrollTo(0, 0)
      this.scroll.current = SCROLL_STATE.NEUTRAL
      this.scroll.buffer = 0
    },
  },
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.app-scroll__btn {
  position: fixed;
  bottom: 10px;
  right: 10px;
  min-width: 40px;
  width: 40px;
  height: 40px;
  font-size: large;
  border-radius: 0;
  border: none;
  border: 3px solid black;
  background-color: gray;
  color: white;
  cursor: pointer;
}

.chevron::before {
	border-style: solid;
	border-width: 0.25em 0.25em 0 0;
	content: '';
	display: inline-block;
	height: 0.45em;
	position: relative;
  top: 6px;
	transform: rotate(-45deg);
	vertical-align: top;
	width: 0.45em;
}

.slide-down-fade-enter-active {
  transition: all .3s ease;
}
.slide-down-fade-leave-active {
  transition: all .3s ease-in-out;
}
.slide-down-fade-enter, .slide-down-fade-leave-to
/* .slide-fade-leave-active below version 2.1.8 */ {
  transform: translateY(10px);
  opacity: 0;
}

</style>
