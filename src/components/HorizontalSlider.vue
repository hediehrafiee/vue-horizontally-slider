<template lang="pug">
  div.container(ref="container")
    ul.slider(:style="sliderStyle" ref="slider")
      slot
    div.parentButtons(ref="perviousButton" v-if='showPreviousButton')
      slot(name="perviousButton")
    div.parentButtons(ref="nextButton" v-if='showNextButton')
      slot(name="nextButton")
</template>

<script>
const defaultTransition = 'left 1.5s ease'
export default {
  data () {
    return {
      slider: {
        left: 0,
        transition: null
      },
      showPreviousButton: false,
      showNextButton: false,
      sliderMove: false,
      leftStart: 0,
      strartX: 0,
      maxLeft: 0
    }
  },
  computed: {
    sliderStyle () {
      return {
        left: this.slider.left + 'px',
        transition: this.slider.transition,
        top: '0px'
      }
    }
  },
  watch: {
    'slider.left' (newVal) {
      this.showSliderButtons(newVal)
    }
  },
  methods: {
    getContainerWidth () {
      return this.$refs.container.clientWidth
    },
    showSliderButtons (slideLeft) {
      const containerWidth = this.getContainerWidth()
      const sliderWidth = this.$refs.slider.scrollWidth
      if ((sliderWidth > containerWidth && slideLeft === 0) || slideLeft > 0) {
        this.showPreviousButton = false
      } else {
        this.showPreviousButton = true
      }
      if (sliderWidth > containerWidth &&
         (sliderWidth - Math.abs(slideLeft) > containerWidth)) {
        this.showNextButton = true
      } else {
        this.showNextButton = false
      }
    },
    slideLeft () {
      this.slider.transition = defaultTransition
      var containerWidth = this.getContainerWidth()
      this.slider.left += containerWidth
      // start slide
      if (this.slider.left > 0) {
        this.slider.left = 0
      }
    },
    slideRight () {
      const buttonNextWidth = this.$refs.nextButton.children[0].clientWidth
      const containerWidth = this.getContainerWidth()
      this.slider.transition = defaultTransition
      this.slider.left -= containerWidth - buttonNextWidth
      // Don't slide more than slider content
      if (Math.abs(this.slider.left) > this.maxLeft) {
        this.slider.left = -this.maxLeft
      }
    },
    dragStart (e) {
      this.slider.transition = null
      this.sliderMove = true
      if (e.type === 'touchstart') {
        this.strartX = e.touches[0].clientX
        this.leftStart = this.slider.left
      } else if (e.type === 'mousedown') {
        this.strartX = e.clientX
        this.leftStart = this.slider.left
      }
    },
    drag (e) {
      const containerWidth = this.getContainerWidth()
      const sliderWidth = this.$refs.slider.scrollWidth
      var dragX
      var directionSlide

      if (e.type === 'touchmove') {
        dragX = e.touches[0].clientX
      } else if (e.type === 'mousemove') {
        dragX = e.clientX
      }

      if (!this.sliderMove) return

      // Detect right and left slide
      (dragX - this.strartX > 0) ? directionSlide = 'right' : directionSlide = 'left'

      if ((sliderWidth - Math.abs(this.slider.left) > containerWidth) &&
        directionSlide === 'left') {
        this.slider.left = this.leftStart + (dragX - this.strartX)
        // Don't slide more than slider content
        if (Math.abs(this.slider.left) > this.maxLeft) {
          this.slider.left = -this.maxLeft
        }
      } else if (directionSlide === 'right') {
        this.slider.left = this.leftStart + (dragX - this.strartX)
      }
      // start slide
      if (this.slider.left > 0) {
        this.slider.left = 0
      }
    },
    dragEnd (e) {
      this.sliderMove = false
    },
    resizeWindow () {
      this.showSliderButtons(this.slider.left)
      const containerWidth = this.getContainerWidth()
      const sliderWidth = this.$refs.slider.scrollWidth
      this.maxLeft = sliderWidth - containerWidth
    }
  },
  mounted () {
    const containerWidth = this.getContainerWidth()
    const sliderWidth = this.$refs.slider.scrollWidth
    this.maxLeft = sliderWidth - containerWidth

    this.$refs.slider.addEventListener('mousedown', this.dragStart)
    window.addEventListener('mousemove', this.drag)
    window.addEventListener('mouseup', this.dragEnd)

    this.$refs.slider.addEventListener('touchstart', this.dragStart)
    window.addEventListener('touchmove', this.drag)
    window.addEventListener('touchend', this.dragEnd)

    window.addEventListener('resize', this.resizeWindow)
    this.showSliderButtons(this.slider.left)
  },
  beforeDestroy () {
    this.$refs.slider.removeEventListener('mousedown', this.dragStart)
    window.removeEventListener('mousemove', this.drag)
    window.removeEventListener('mouseup', this.dragEnd)

    this.$refs.slider.removeEventListener('touchstart', this.dragStart)
    window.removeEventListener('touchmove', this.drag)
    window.removeEventListener('touchend', this.dragEnd)

    window.removeEventListener('resize', this.resizeWindow)
  }
}
</script>

<style scoped lang="stylus">
.container
  position: relative
  min-height: 70px
  overflow: hidden

ul.slider
  white-space: nowrap
  padding: 0
  position: absolute
  cursor: pointer

.parentButtons
  position: relative

.previousButton
  position: absolute
  left: 0px
  border-bottom-right-radius: 36px
  border-top-right-radius: 36px
  box-shadow: 1px 0 6px rgba(0,0,0,0.2)

.nextButton
  position: absolute
  right: 0px
  border-bottom-left-radius: 36px
  border-top-left-radius: 36px
  box-shadow: -1px 0 6px rgba(0,0,0,0.2)

button
  background-color: white
  width: 36px
  cursor: pointer
  height: 65px
  color: black
  border-right: none
  border-bottom: none
  border-left: none
  top: 0px
  z-index: 1
  display: block
  visibility: inherit
  transition: opacity 0.5s,visibility 0.5s
  outline: none

</style>
