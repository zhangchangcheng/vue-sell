<template>
  <div class="ratings" rel="ratings">
    <div class="ratings-content">
      <div class="overview">
        <div class="overview-left">
          <h1 class="score">{{seller.score}}</h1>
          <div class="title">综合评分</div>
          <div class="rank">高于周边商家{{seller.rankRate}}%</div>
        </div>
        <div class="overview-right">
          <div class="score-wrapper">
            <span class="title">服务态度</span>
            <star :size="36" :score="seller.serviceScore"></star>
            <span class="score">{{seller.serviceScore}}</span>
          </div>
          <div class="score-wrapper">
            <span class="title">商品评分</span>
            <star :size="36" :score="seller.foodScore"></star>
            <span class="score">{{seller.foodScore}}</span>
          </div>
          <div class="delivery-wrapper">
            <span class="title">送达时间</span>
            <span class="delivery">{{seller.deliveryTime}}分钟</span>
          </div>
        </div>
      </div>
      <split></split>
      <rating-select :select-type="selectType" :only-content="onlyContent" :desc="desc"
            :ratings="food.ratings"
            @toggleContent="toggleContentParent" @selectType="selectTypeParent"
          ></rating-select>
      <div class="rating-wrapper">
        <ul>
          <li v-for="(rating,inx) in ratings" :key="inx" v-show="needShow(rating.rateType, rating.text)" class="rating-item">
            <div class="avatar">
              <img width="28" height="28" :src="rating.avatar">
            </div>
            <div class="content">
              <h1 class="name">{{rating.username}}</h1>
              <div class="star-wrapper">
                <star :size="24" :score="rating.score"></star>
                <span class="delivery" v-show="rating.deliveryTime">{{rating.deliveryTime}}</span>
              </div>
              <p class="text">{{rating.text}}</p>
              <div class="recommend" v-show="rating.recommend && rating.recommend.length">
                <span class="icon-thumb_up"></span>
                <span class="item" v-for="(item,inx) in rating.recommend" :key="inx">{{item}}</span>
              </div>
              <div class="time">
                {{rating.rateTime | formatDate}}
              </div>
            </div>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import {formatDate} from '@/common/js/date'
import BScroll from 'better-scroll'
import Star from '@/components/star/star'
import Split from '@/components/split/Split'
import RatingSelect from '@/components/ratingselect/RatingSelect'

const ALL = 2
const ERR_OK = 0

export default {
  props: {
    seller: {
      type: Object
    }
  },
  components: {
    Star,
    Split,
    RatingSelect
  },
  data() {
    return {
      ratings: [],
      selectType: ALL,
      onlyContent: true
    }
  },
  created() {
    this.$http.get('api/ratings').then((res) => {
      res = res.data
      if (res.data.errno === ERR_OK) {
        this.ratings = res.data
        this.nextTick(() => {
          this.scroll = new BScroll(this.$rels.ratings, {
            click: true
          })
        })
      }
    })
  },
  methods: {
    selectTypeParent(type) {
      this.selectType = type
      this.$nextTick(() => {
        this.scroll.refresh()
      })
    },
    toggleContentParent(onlyContent) {
      this.onlyContent = onlyContent
      this.$nextTick(() => {
        this.scroll.refresh()
      })
    }
  },
  filters: {
    formatDate(time) {
      let date = new Date(time)
      return formatDate(date, 'yyyy-MM-dd hh:mm')
    }
  }
}
</script>

<style lang="scss">
  .ratings {
    position: absolute;
    top: 17px;
    bottom: 0;
    left: 0;
    width: 100%;
    overflow: hidden;
    .overview {
      display: flex;
      padding: 18px 0;
      .overview-left {
        flex: 0 0 137px;
        width: 137px;
        padding-bottom: 6px;
        border-right: 1px solid rgba(7, 17, 27, 0.1);
        text-align: center;
        .score {
          margin-bottom: 6px;
          line-height: 28px;
          font-size: 24px;
          color: rgb(255, 153, 0);
        }
        .title {
          line-height: 12px;
          font-size: 12px;
          color: rgb(7, 17, 27);
        }
        .rank {
          line-height: 10px;
          font-size: 10px;
          color: rgb(147, 153, 159);
        }
      }
      .overview-right {
        flex:1;
        padding-left: 24px;
      }
    }
  }
</style>
