<template>
<div class="pulpul">
  <div class="body"
    :style="{ transform: `translateY(${diff}px)` }"
  >
    <section class="spinner top"> 下拉刷新 </section>

    <slot></slot>

    <section class="spinner bottom"> 上拉加载 </section>
  </div>
</div>
</template>

<script>
export default {
  props: {
    topMethod: {
      type: Function,
      default: () => ({})
    },

    bottomMethod: {
      type: Function,
      default: () => ({})
    },

    distance2Fire: {
      type: Number,
      default: 100,
    },
  },

  computed: {
    diff() {
      return this.isTouchEnd ? 0 : this.end - this.start
    }, 
  },
  
  data() {
    return {
      start: void 2,
      end: void 2,
      isTouchEnd: false,
    }
  },
  
  mounted() {
    this.$el.addEventListener('touchstart', this.whenTouchStart)
    this.$el.addEventListener('touchmove', this.whenTouchMove) 
    this.$el.addEventListener('touchend', this.whenTouchEnd) 
  },
  
  methods: {
    whenTouchStart ({ targetTouches }) {
      this.isTouchEnd = false
      this.start = targetTouches[0].pageY
    },

    whenTouchMove ({ targetTouches }) {
      this.end = targetTouches[0].pageY
    },
    
    whenTouchEnd ({ targetTouches }) {
      this.diff >= this.distance2Fire
        ? this.topMethod()
        : -this.diff >= this.distance2Fire
          ? this.bottomMethod()
          : null

      this.isTouchEnd = true
      this.start = this.end = void 2
    }
  },
}
</script>

<style scoped>
.pulpul {
  overflow: hidden;
  font-size: 3rem;
}

.spinner {
  height: 50px;
  line-height: 50px;
  text-align: center;
  font-size: 2rem;
}
  
.spinner.top {
  margin-top: -50px;
}
.spinner.bottom {
  margin-bottom: -50px;
}
</style>