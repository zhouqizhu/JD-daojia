<template>
    <div class="wrapper">
        <div class="search">
            <div
                class="search__back iconfont" @click="handleBackClick"
            >&#xe600;</div>
            <div class="search__content">
                <span class="search__content__icon iconfont">&#xe6ac;</span>
                <input class="search__content__input" placeholder="请输入商品名称">
            </div>
        </div>
        <ShopInfo :item="item" :hideBorder='true' />
    </div>
</template>

<script>
import { useRouter } from 'vue-router'
import ShopInfo from '../../components/ShopInfo.vue'
export default {
  name: 'Shop',
  components: { ShopInfo },
  setup () {
    const router = useRouter()
    const item = {}
    // 点击回退
    const handleBackClick = () => {
      router.back()
    }
    return { item, handleBackClick }
  }
}
</script>

<style lang="scss" scoped>
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
        background: #f5f5f5;
        border-radius: .16rem;
        &__icon {
            width: .44rem;
            text-align: center;
            background: #b7b7b7;
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
            color: #333;
            &::placeholder {
                color: #333;
            }
        }
    }
}
</style>
