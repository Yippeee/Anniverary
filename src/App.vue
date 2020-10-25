<template>
<div class="love-wrap">
  <CustomerImg />
  <CustomerImg />

</div>
</template>

<script>
import CustomerImg from './components/CustomerImg.vue'

export default {
  name: 'App',
  components: {
    CustomerImg
  }
}
</script>
<style>
#app{
  display:flex;
  justify-content: center;
  align-items: center;
}
.love-wrap{
  height: 520px;
  width: 1314px;
  border: 1px solid gray;
}
</style>