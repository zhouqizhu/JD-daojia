<template>
    <div class="wrapper">
        <div class="search">
            <div class="search__back iconfont" @click="handleBackClick">
                &#xe600;
            </div>
            <div class="search__content">
                <span class="search__content__icon iconfont">&#xe6ac;</span>
                <input class="search__content__input" placeholder="请输入商品名称">
            </div>
        </div>
        <ShopInfo :item="item" :hideBorder='true' v-show="item.imgUrl" />
        <Content :shopName="item.name" />
        <Cart />
    </div>
</template>

<script>
import { reactive, toRefs } from '@vue/reactivity'
import { useRoute, useRouter } from 'vue-router'
import ShopInfo from '../../components/ShopInfo.vue'
import { get } from '../../utils/request'
import Content from '../shop/Content.vue'
import Cart from '../shop/Cart.vue'

// 获取当前商品信息
const useShopInfoEffect = () => {
  // 获取当前路由
  const route = useRoute()
  const data = reactive({ item: {} })
  const getItemData = async () => {
    const result = await get(`/api/shop/${route.params.id}`)
    if (result?.errno === 0 && result?.data) {
      data.item = result.data
    }
  }
  const { item } = toRefs(data)
  return { item, getItemData }
}
// 回退逻辑
const useBackRouterEffect = () => {
  const router = useRouter()
  // 点击回退
  const handleBackClick = () => {
    router.back()
  }
  return handleBackClick
}
export default {
  name: 'Shop',
  components: { ShopInfo, Content, Cart },
  setup () {
    const { item, getItemData } = useShopInfoEffect()
    const handleBackClick = useBackRouterEffect()
    getItemData()

    return { item, handleBackClick }
  }
}
</script>

<style lang="scss" scoped>
@import '../../style/viriables.scss';
.wrapper {
    padding: 0 .18rem;
}
.search {
    display: flex;
    margin: .14rem 0 .04rem 0;
    line-height: .32rem;
    &__back {
        width: .3rem;
        font-size: .24rem;
        color: #b6b6b6;
    }
    &__content {
        display: flex;
        flex: 1;
        background: $search-bgColor;
        border-radius: .16rem;
        &__icon {
            width: .44rem;
            text-align: center;
            background: $search-fontColor;
        }
        &__input {
            display: block;
            width: 100%;
            border: none;
            padding-right: .2rem;
            outline: none;
            background: none;
            height: .32rem;
            font-size: .14rem;
            color: $content-fontcolor;
            &::placeholder {
                color: $content-fontcolor;
            }
        }
    }
}
</style>
