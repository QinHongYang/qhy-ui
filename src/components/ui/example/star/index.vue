<template>
  <div class="rate" :class="{'disabled':disabled}">
    <span v-if="showText" class="text">{{curScore||score}}分</span>
    <div class="star-wrap">
      <i v-for="(i, index) in 5"
        :key="index"
        @mouseenter="disabled ? '' : curScore=i"
        @mouseleave="disabled ? '' : curScore=''"
        @click="disabled ? '' : setScore(i)"
        :class="getClass(i)">
        <i v-if="disabled && i===Math.floor(score)+1" class="icon-star" :style="'width:'+width"></i>
      </i>
    </div>
  </div>
</template>
<script>
export default {
  name: 'q-star',
  props: {
    // 分数，默认0，保留一位小数
    score: {
      type: Number,
      default: 0
    },
    // 是否只读，默认false，鼠标点击可以打分
    disabled: {
      type: Boolean,
      default: false
    },
    // 是否显示分数，默认false
    showText: {
      type: Boolean,
      default: false
    }
  },
  data () {
    return {
      curScore: '',
      width: ''
    }
  },
  created: function () {
    this.getDecimal()
  },
  methods: {
    getClass (i) {
      if (this.curScore === '') {
        return i <= this.score ? 'icon-star' : 'icon-star-o'
      } else {
        return i <= this.curScore ? 'icon-star' : 'icon-star-o'
      }
    },
    getDecimal () {
      this.width = Number(this.score * 100 - Math.floor(this.score) * 100) + '%'
    },
    setScore (i) {
      this.$emit('update:score', i)
    }
  }
}
</script>
<style lang="scss" scoped>
.rate{
  .text{
    font-size: 18px;
    color: #ff7f2c;
    font-weight: bold;
  }
  .star-wrap{
    line-height: 0;
    .icon-star-o{
      position: relative;
      width:1.5rem;
      height: 1.5rem;
      line-height: 0;
      display: inline-block;
      margin-right: 2px;
      background: url('../../img/star/icon-star-gray.png') no-repeat 0 0;
      background-size:100% 100%;
      .icon-star{
        position: absolute;
        left:0;
        top:0;
      }
    }
    .icon-star{
      width:1.5rem;
      height: 1.5rem;
      line-height: 0;
      display: inline-block;
      margin-right: 2px;
      background: url('../../img/star/icon-star-yellow.png') no-repeat 0 0;
      background-size:100% 100%;
    }
    i:last-child{
      margin-right: 0;
    }
  }
}
</style>
