<template>
  <div class="docker">
    <div v-for="(item, index) in dockerList" :key="item.icon"
      :class="{'docker__item': true, 'docker__item--active': index === 0}"
    >
      <div class="iconfont" v-html="item.icon" />
      <div class="docker__title">{{item.text}}</div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Docker',
  setup () {
    const dockerList = [
      { icon: '&#xe6b8;', text: '首页' },
      { icon: '&#xe746;', text: '购物车' },
      { icon: '&#xe610;', text: '订单' },
      { icon: '&#xe78b;', text: '我的' }
    ]
    return { dockerList }
  }
}
</script>

<style lang="scss" scoped>
@import '../../style/viriables.scss';
.docker {
  display: flex;
  box-sizing: border-box;
  position: absolute;
  left: 0;
  bottom: 0;
  padding: 0 .18rem;
  width: 100%;
  height: .49rem;
  border-top: .01rem solid #f1f1f1;
  color: $content-fontcolor;
  &__item {
  flex: 1;
  text-align: center;
  .iconfont {
    font-size: .18rem;
    margin: .07rem 0 .02rem 0;
  }
  &--active {
    color: #1fa4fc;
  }
  }
  &__title {
    font-size: .2rem;
    transform: scale(.5, .5);
    transform-origin: center top;
  }
}
</style>
