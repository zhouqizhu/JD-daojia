<template>
    <div class="content">
        <div class="category">
            <div :class="{'category__item': true, 'category__item--active': currentTab === item.name}"
            v-for="item in categories"
            :key="item.name"
            @click="() => handleCategoryClick(item.tab)"
            >
            {{item.name}}
            </div>
        </div>
        <div class="product">
            <div class="prduct__item" v-for="item in list" :key="item._id">
                <img class="product__item__img" src="" alt="">
                <div class="product__item__detail">
                    <h4 class="product__item">{{item.name}}</h4>
                    <p class="product__item__sales">{{item.sales}}</p>
                    <p class="product__item__price">
                      <span class="product__item__yen"></span>{{item.price}}
                      <span class="product__item__origin">{{item.oldPrice}}</span>
                    </p>
                </div>
            </div>
            <div class="product__number">
              <span class="product__number__minus">-</span>
              <span class="priduct__number__plus">+</span>
            </div>
        </div>
    </div>
</template>

<script>
import { reactive, ref, toRefs } from '@vue/reactivity'
import { get } from '../../utils/request'
import { watchEffect } from '@vue/runtime-core'

const categories = [
  { name: '全部商品', tab: 'all' },
  { name: '秒杀', tab: 'seckill' },
  { name: '新鲜水果', tab: 'fruit' }
]
// 切换tab
const useTabEffect = () => {
  const currentTab = ref(categories[0].tab)
  const handleTabClick = (tab) => {
    currentTab.value = tab
  }
  return { currentTab, handleTabClick }
}
// 列表内容相关逻辑
const useCurrentListEffect = (currentTab, shopId) => {
  const content = reactive({ list: [] })
  const getContentData = async () => {
    const result = await get(`/api/shop/${shopId}/products`, { tab: currentTab.value })
    if (result?.errno === 0 && result?.data.length) {
      content.list = result.data
    }
  }
  watchEffect(() => { getContentData() })
  const { list } = toRefs(content)
  return { list }
}
export default {
  name: 'Content',
  setup () {
    const { currentTab, handleTabClick } = useTabEffect()
    const { list } = useCurrentListEffect(currentTab)
    return { list, currentTab, handleTabClick, categories }
  }
}
</script>

<style lang="scss" scoped>
@import '../../style/viriables.scss';
@import '../../style/mixins.scss';
.content {
  display: flex;
  position: absolute;
  left: 0;
  right: 0;
  top: 1.5rem;
  bottom: .5rem;
}
.category {
  overflow-y: scroll;
  height: 100%;
  width: .76rem;
  background: $search-bgColor;
  &__item {
    line-height: .4rem;
    text-align: center;
    font-size: .14rem;
    color: $content-fontcolor;
    &--active {
      background: $bgColor;
    }
  }
}
.product {
  overflow-y: scroll;
  flex: 1;
  &__item {
    position: relative;
    display: flex;
    padding: .12rem 0;
    margin: 0 .16rem;
    border-bottom: .01rem solid $content-bgColor;
    &__detail {
      overflow: hidden;
    }
    &__img {
      width: .68rem;
      height: .68rem;
      margin-right: .16rem;
    }
    &__title {
      margin: 0;
      line-height: .2rem;
      font-size: .14rem;
      color: $content-fontcolor;
      @include ellipsis;
    }
    &__sales {
      margin: .06rem 0;
      font-size: .12rem;
      color: $content-fontcolor;
    }
    &__price {
      margin: 0;
      line-height: .2rem;
      font-size: .14rem;
      color: $hightlight-fontColor;
    }
    &__yen {
      font-size: .12rem;
    }
    &__origin {
      margin-left: .06rem;
      line-height: .2rem;
      font-size: .12rem;
      color: $light-fontColor;
      text-decoration: line-through;
    }
    .product__number {
      position: absolute;
      right: 0;
      bottom: .12rem;
      line-height: .18rem;
      &__minus {
        position: relative;
        top: .02rem;
        color: $medium-fontColor;
        margin-right: .05rem;
      }
      &__plus {
        position: relative;
        top: .02rem;
        color: $btn-bgColor;
        margin-left: .05rem;
      }
    }
  }
}
</style>
