<template lang="pug">
  div(class='home')
    HorizontalSlider(ref="horizontallySlider")
      horizontalSliderItem(v-for="(item, index) in sliderItems" :key="index")
        | {{ item.title }}
      template(v-slot:perviousButton)
        button.previousButton(@click="slideLeft()") P
      template(v-slot:nextButton)
        button.nextButton(@click="slideRight()" ) N
</template>

<script>

import HorizontalSlider from '@/components/HorizontalSlider.vue'
import horizontalSliderItem from '@/components/horizontal-slider-item.vue'

export default {
  name: 'home',
  components: {
    HorizontalSlider,
    horizontalSliderItem
  },
  data () {
    return {
      sliderItems: [
        {
          title: 'Learn vuejs'
        },
        {
          title: 'Learn stylus'
        },
        {
          title: 'hi'
        },
        {
          title: 'myself'
        },
        {
          title: 'yourself'
        },
        {
          title: 'tommorrow'
        },
        {
          title: 'afternoon'
        },
        {
          title: 'night'
        },
        {
          title: 'day'
        },
        {
          title: 'nextday'
        },
        {
          title: 'nextyear'
        },
        {
          title: 'dog'
        },
        {
          title: 'pug'
        },
        {
          title: 'cat'
        },
        {
          title: 'how are you'
        },
        {
          title: 'Hedieh'
        },
        {
          title: 'Rafiee'
        },
        {
          title: 'sell phone'
        },
        {
          title: 'good bye'
        },
        {
          title: 'fish'
        },
        {
          title: 'onion'
        },
        {
          title: 'lion'
        },
        {
          title: 'panda'
        },
        {
          title: 'Learn vuejs'
        },
        {
          title: 'Learn stylus'
        },
        {
          title: 'hi'
        },
        {
          title: 'myself'
        },
        {
          title: 'yourself'
        },
        {
          title: 'tommorrow'
        },
        {
          title: 'afternoon'
        },
        {
          title: 'night'
        },
        {
          title: 'day'
        },
        {
          title: 'nextday'
        },
        {
          title: 'nextyear'
        },
        {
          title: 'dog'
        },
        {
          title: 'pug'
        },
        {
          title: 'cat'
        },
        {
          title: 'how are you'
        },
        {
          title: 'Hedieh'
        },
        {
          title: 'Rafiee'
        },
        {
          title: 'sell phone'
        },
        {
          title: 'good bye'
        },
        {
          title: 'fish'
        },
        {
          title: 'onion'
        },
        {
          title: 'lion'
        },
        {
          title: 'panda'
        }
      ]
    }
  },
  methods: {
    slideLeft () {
      this.$refs.horizontallySlider.slideLeft()
    },
    slideRight () {
      this.$refs.horizontallySlider.slideRight()
    }
  }
}
</script>
<style>
  .home {
    height: 50vh;
  }
</style>
