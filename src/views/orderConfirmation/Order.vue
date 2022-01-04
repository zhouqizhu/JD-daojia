<template>
    <div class="order">
        <div class="order__price">付款金额<b>￥{{calculations.price}}</b></div>
        <div class="order__btn" @click="handleShowConfirmChange(true)">提交订单</div>
    </div>
    <div class="mask" v-show="showConfirm" @click="handleShowConfirmChange(false)">
        <div class="mask__content" @click.stop>
            <h3 class="mask__content__title">确认要离开收银台？</h3>
            <p class="mask__content__desc">请尽快完成支付，否则将被取消</p>
            <div class="mask__content__btns">
                <div
                    class="mask__content__btn mask__content__btn--first"
                    @click="() => handleConfirmOrder(true)"
                >取消订单</div>
                <div
                    class="mask__content__btn mask__content__btn--last"
                    @click="() => handleConfirmOrder(false)"
                >确认支付</div>
            </div>
        </div>
    </div>
</template>

<script>
import { useRoute, useRouter } from 'vue-router'
import { useCommonCartEffect } from '../../effects/cartEffects'
import { post } from '../../utils/request'
import { useToastEffect } from '../../components/Toast.vue'
import { useStore } from 'vuex'
import { ref } from '@vue/reactivity'

// 和确认订单相关的逻辑
const useMakeOrderEffect = (shopId, shopName, productList) => {
  const router = useRouter()
  const store = useStore()
  const { showToast } = useToastEffect()

  const handleConfirmOrder = async (isCanceled) => {
    const products = []
    for (const i in productList.value) {
      const product = productList.value[i]
      products.push({ id: parseInt(product._id, 10), num: product.count })
    }
    try {
      const result = await post('/api/order', {
        addressId: 1, shopId, shopName: shopName.value, isCanceled, products
      })
      if (result?.errno === 0) {
        store.commit('clearCartData', shopId)
        router.push({ name: 'OrderList' })
      }
    } catch (e) {
      showToast('下单失败')
    }
  }
  return handleConfirmOrder
}

// 确认订单蒙层展示相关逻辑
const useShowMaskEffect = () => {
  const showConfirm = ref(false)
  const handleShowConfirmChange = (status) => {
    showConfirm.value = status
  }
  return { handleShowConfirmChange, showConfirm }
}

export default {
  name: 'Order',
  setup () {
    const route = useRoute()

    const shopId = parseInt(route.params.id, 10)
    const { calculations, shopName, productList } = useCommonCartEffect(shopId)

    const { handleConfirmOrder } = useMakeOrderEffect(shopId, shopName, productList)
    const { handleShowConfirmChange, showConfirm } = useShowMaskEffect()

    return { calculations, showConfirm, handleShowConfirmChange, handleConfirmOrder }
  }
}
</script>

<style lang="scss" scoped>
@import '../../style/viriables.scss';
.order {
    display: flex;
    height: .49rem;
    line-height: .49rem;
    background: $bgColor;
    position: absolute;
    left: 0;
    right: 0;
    bottom: 0;
    width: 100%;
    bottom: 0;
    &__price {
        flex: 1;
        text-indent: .24rem;
        font-size: .14rem;
        color: $content-fontcolor;
        text-align: left;
    }
    &__btn {
        width: .98rem;
        background: #4fb0f9;
        color: $bgColor;
        text-align: center;
        font-size: .14rem;
    }
}
.mask {
    z-index: 1;
    position: absolute;
    left: 0;
    right: 0;
    bottom: 0;
    top: 0;
    background: rgba(0, 0, 0, 0.50);
    &__content {
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
        width: 3rem;
        height: 1.56rem;
        background: $bgColor;
        border-radius: .04rem;
        text-align: center;
        &__title {
            margin: .24rem 0 0 0;
            line-height: .26rem;
            font-size: .18rem;
            color: $content-fontcolor;
        }
        &__desc {
            font-size: .14rem;
            color: $medium-fontColor;
            margin: .08rem 0 0 0;
        }
        &__btns {
            margin: .24rem .58rem;
            display: flex;
        }
        &__btn {
            flex: 1;
            width: .8rem;
            line-height: .32rem;
            border-radius: .16rem;
            font-size: .14rem;
            &--first {
                margin-right: .12rem;
                border: .01rem solid #4fb0f9;
                color: #4fb0f9;
            }
            &--last {
                margin-left: .12rem;
                background: #4fb0f9;
                color: $bgColor;
            }
        }
    }
}
</style>
