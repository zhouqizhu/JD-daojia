<template>
<div class="wrapper">
    <TopArea />
    <ProductList />
    <!-- 底部提交订单 -->
    <Order />
</div>
</template>

<script>
import TopArea from './TopArea'
import ProductList from './ProductList'
import Order from './Order'
export default {
  name: 'OrderConfirmation',
  components: { TopArea, ProductList, Order }
}
</script>

<style lang="scss" scoped>
@import '../../style/viriables.scss';
.wrapper {
    position: absolute;
    left: 0;
    right: 0;
    top: 0;
    bottom: 0;
    background-color: #eee;
    overflow-y: scroll;
}

</style>
