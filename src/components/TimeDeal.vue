<template>
  <section class="time-deal">
    <div
      ref="lazyLoadElement"
      class="inner">
      <h2>타임딜</h2>

      <div class="swiper-outer">
        <div
          ref="swiper"
          class="swiper-container">
          <div class="swiper-wrapper">
            <div
              v-for="product in products"
              :key="product.name"
              class="swiper-slide">
              <time-deal-card
                :timedeal="product"></time-deal-card>
            </div>
          </div>
        </div>
        <div
          v-show="isShowPrevButton"
          class="prev"></div>
        <div
          class="next"
          @click="clickNextButton"></div>
      </div>
    </div>
  </section>
</template>

<script>
import TimeDealCard from '~/src/components/TimeDealCard'
import Swiper from 'swiper/bundle'
import 'swiper/swiper-bundle.css'

export default {
  components: {
    TimeDealCard
  },
  data() {
    return {
      products: [],
      isShowPrevButton : false
    }
  },
  mounted() {
    this.init()
  },
  methods: {
    async init () {
      this.$lazyLoad(this.$refs.lazyLoadElement)
      this.products = await this.$fetch({
        requestName: 'hotFocus'
      })

      this.$nextTick(() => {
        new Swiper(this.$refs.swiper, {
          speed: 1000,
          loop:true,
          navigation: {
            nextEl: '.time-deal .next',
            prevEl: '.time-deal .prev'
          }
        })
      })
    },
    clickNextButton() {
      this.isShowPrevButton = true
    }
  }
}
</script>

<style scoped lang="scss">
section {
  background-color: #ffffff;
  .inner {
    height: 612px;
  }

  .swiper-outer {
    position: relative;
    .prev,
    .next {
      position: absolute;
      top: 50%;
      width: 50px;
      height: 50px;
      margin-top: -25px;
      border-radius: 50%;
      box-shadow: 0 4px 16px rgba(#000, .15);
      background-color: #fff;
      z-index: 1;
      display: flex;
      justify-content: center;
      align-items: center;
      cursor: pointer;
      outline: none;
      &::after {
        content: "";
        width: 10px;
        height: 18px;
        background-image: url("https://trusting-williams-8cacfb.netlify.app/images/main_2x.png");
        background-size: 209px;
      }
    }
    .prev {
      left: -25px;
      &::after {
        background-position: -199px 0;
      }
    }
    .next {
      right: -25px;
      &::after {
        background-position: -199px -46px;
      }
    }
  }
}
</style>
