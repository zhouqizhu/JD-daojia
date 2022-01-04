<template>
    <div class="wrapper">
        <div class="title">我的订单</div>
        <div class="orders">
            <div class="order" v-for="(item, index) in list" :key="index">
              <div class="order__title">
                {{item.shopName}}
                <span class="order__status">{{item.isCanceled ? '已取消' : '已下单'}}</span>
              </div>
              <div class="order__content">
                <div class="order__content__imgs">
                  <template v-for="(innerItem, innerIndex) in item.products" :key="innerIndex">
                    <img
                    class="order__content__img" src="innerItem.product.img"
                    v-if="innerIndex <= 3">
                  </template>
                </div>
                <span class="order__content__info">
                  <div class="order__content__info__price">{{item.totalPrice}}</div>
                  <div class="order__content__info__count">共 {{item.totalNumber}} 件</div>
                </span>
              </div>
            </div>
        </div>
    </div>
    <Docker :currentIndex='2' />
</template>

<script>
import { reactive, toRefs } from '@vue/reactivity'
import Docker from '../../components/Docker'
import { get } from '../../utils/request'

// 处理订单列表逻辑
const useOrderListEffect = () => {
  const data = reactive({ list: [] })
  const getNearbyList = async () => {
    const result = await get('/api/order')
    if (result?.errno === 0 && result?.data?.length) {
      const tempData = result.data
      tempData.forEach(order => {
        const products = order.products || []
        let totalPrice = 0
        let totalNumber = 0
        products.forEach(productItem => {
          totalNumber += (productItem?.olderSales || 0)
          totalPrice += ((productItem?.product?.price * productItem?.olderSales) || 0)
        })
        order.totalPrice = totalPrice
        order.totalNumber = totalNumber
      })
      data.list = result.data
    }
  }
  getNearbyList()

  const { list } = toRefs(data)
  return { list }
}
export default {
  name: 'OrderList',
  components: { Docker },
  setup () {
    const { list } = useOrderListEffect()
    return { list }
  }
}
</script>

<style lang="scss" scoped>
@import '../../style/viriables.scss';
.wrapper {
  overflow-y: auto;
  position: absolute;
  left: 0;
  top: 0;
  bottom: .5rem;
  right: 0;
  background: #F8F8F8;
}
.title {
    line-height: .44rem;
    background: $bgColor;
    font-size: .16em;
    color: $content-fontcolor;
    text-align: center;
}
.order {
  margin: .16rem .18rem;
  padding: .16rem;
  background: $bgColor;
  &__title {
    line-height: .22rem;
    font-size: .16rem;
    color: $content-fontcolor;
    margin-bottom: .16rem;
  }
  &__status {
    float: left;
    font-size: .14rem;
    color: #999;
  }
  &__content {
    display: flex;
    &__imgs {
      flex: 1;
    }
    &__img {
      width: .4rem;
      height: .4rem;
      margin-right: .12rem;
    }
    &__info {
      width: .7rem;
    }
    &__price {
      font-size: .14rem;
      color: #e93b3b;
      text-align: right;
      line-height: .2rem;
      margin-bottom: .04rem;
    }
    &__count {
      font-size: .12rem;
      color: $content-fontcolor;
      text-align: right;
      line-height: .14rem;
    }
  }
}
</style>
