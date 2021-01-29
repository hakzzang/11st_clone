<template>
  <div class="time-deal-card">
    <div class="product-contents">
      <div
        :class="{ loading: imageLoading }"
        class="thumbnail"
        :style="{ backgroundImage: `url(${timedeal.thumbnail})` }"></div>

      <div class="contents">
        <div class="content_timer">
          {{ timeString }}
        </div>
        <div class="flag">
          {{ timedeal.flag }}
        </div>
        <div class="name">
          {{ timedeal.name }}
        </div>
        <div class="price_content">
          <div class="price_suffix">
            타임딜가
          </div>
          <div class="prices">
            <div class="price">
              <span class="value">
                {{ addComma(timedeal.price.value) }}
              </span>
              <span class="unit">
                {{ timedeal.price.unit }}
              </span>
            </div>

            <div
              v-if="timedeal.listPrice"
              class="list-price">
              <span class="value">
                {{ addComma(timedeal.listPrice.value) }}
              </span>
              <span class="unit">
                {{ timedeal.listPrice.unit }}
              </span>
            </div>
          </div>
        </div>
        <div class="options">
          <div
            v-if="timedeal.options.shipping"
            class="free-shipping">
            {{ timedeal.options.shipping }}
          </div>
          <div
            v-if="timedeal.options.accumulation"
            class="accumulation">
            ㆍ <span v-html="timedeal.options.accumulation"></span>
          </div>
          <span class="flex-space"></span>
          <div
            v-if="timedeal.options.count"
            class="count">
            · {{ timedeal.options.count }}
          </div>
        </div>
        <div class="recommend_flag">
          11번가 추천상품
        </div>
      </div>
    </div>
    <div
      v-if="navigations.exhibitions"
      class="exhibitions">
      <a :href="navigations.exhibitions.href">
        <img
          :src="navigations.exhibitions.src"
          :alt="navigations.exhibitions.name" />
      </a>
    </div>
  </div>
</template>
<script>
import numeral from 'numeral'

export default {
  props: {
    timedeal: {
      type: Object,
      required: true
    }
  },
  data() {
    return {
      navigations: {},
      imageLoading: true,
      timerCount: 14400,
      timeString: ""
    }
  },
  computed: {
    discountRate() {
      if (!this.timedeal.listPrice) return ''

      const price = this.timedeal.price.value
      const listPrice = this.timedeal.listPrice.value
      return Math.floor(100 - (price * 100 / listPrice))
    }
  },
  created() {
    this.init()
  },
  mounted() {
    this.loadImage()
  },
  watch: {
    timerCount: {
      handler(value) {
        setTimeout(() => {
          const secs = this.timerCount
          const hours = Math.floor(secs / (60 * 60));
          const divisor_for_minutes = secs % (60 * 60);
          const minutes = Math.floor(divisor_for_minutes / 60);
          const divisor_for_seconds = divisor_for_minutes % 60;
          const seconds = Math.ceil(divisor_for_seconds);
          this.timeString = numeral(hours).format('00') + ":" + numeral(minutes).format('00') + ":" + numeral(seconds).format('00');
          this.timerCount--;
        }, 1000)
      }, immediate: true
    }
  },
  methods: {
    async init() {
      this.navigations = await this.$fetch({
        requestName: 'navigations'
      })
      this.done = true
    },
    addComma(num) {
      return numeral(num).format('0,0')
    },
    loadImage() {
      const img = document.createElement('img')
      img.src = this.timedeal.thumbnail
      img.addEventListener('load', () => {
        this.imageLoading = false
      })
    }
  }
}
</script>
<style scoped lang="scss">
.time-deal-card {

  display: flex;
  flex-direction: column;
  width: auto;
  height: 402px;
  overflow: hidden;
  box-shadow: 0 2px 10px rgba(#000, 0.06);

  .product-contents {
    display: flex;
    flex-direction: row;
    width: auto;
    height: 302px;
    overflow: hidden;


    .thumbnail {
      width: 605px;
      height: 302px;
      background-repeat: no-repeat;
      background-position: center;
      background-size: cover;

      &.loading {
        display: flex;
        justify-content: center;
        align-items: center;
        background-color: #f4f4f4;

        &::after {
          content: "";
          width: 94px;
          height: 40px;
          margin: 0 24px;
          background-image: url("https://trusting-williams-8cacfb.netlify.app/images/globals_2x.png");
          background-position: -162px 0;
          background-size: 363px;
          filter: grayscale(100%);
          opacity: .2;
        }
      }
    }

    .contents {
      display: inline-block;
      position: relative;
      align-items: center;
      margin: auto;
      padding: 20px 20px 0;
      width: 605px;
      height: 302px;
      box-sizing: border-box;
      background-color: #fff;
      border-top: 1px solid #ececec;
      border-bottom: 1px solid #ececec;

      .content_timer {
        font-size: 28px;
        color: #f43142;
        margin-bottom: 20px;
      }

      .flag {
        display: inline-block;
        position: relative;
        margin: auto;
        padding: 0 4px;
        height: 22px;
        line-height: 22px;
        font-size: 12px;
        color: #6423FF;
        background-color: rgba(100, 35, 255, 0.04);
        border: 1px solid rgba(100, 35, 255, 0.4);
      }

      .name {
        position: relative;
        margin-top: 8px;
        font-size: 19px;
        white-space: nowrap;
      }

      .price_content {
        display: flex;
        flex-direction: row;
        margin-top: 8px;
        align-items: center;

        .price_suffix {
          font-size: 20px;
          color: #f43142;
          margin-right: 12px;
        }
      }

      .prices {
        display: flex;
        align-items: baseline;
        margin-right: 10px;
        color: #333333;

        .value {
          font-size: 24px;

        }
      }

      .price {
        display: flex;
        align-items: baseline;
        margin-right: 4px;

        .value {
          font-size: 24px;
          font-weight: 900;
        }
      }

      .list-price {
        font-size: 16px;
        display: flex;
        align-items: baseline;
        color: #999;
        text-decoration: line-through;
        .value {
          font-size: 16px;
          font-weight: 400;
        }
      }
    }

    .options {
      display: flex;
      font-size: 13px;
      color: #666666;

    }

    .recommend_flag {
      display: flex;
      font-size: 13px;
      margin-top: 48px;
      color: #666666;
    }
  }

  .exhibitions {
    width: 100%;
    height: 100px;
    margin-top: 36px;

    a {
      display: block;
      width: inherit;
      height: inherit;
      cursor: pointer;

      img {
        width: inherit;
        height: inherit;
      }
    }
  }
}
</style>