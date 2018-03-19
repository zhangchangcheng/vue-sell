<template>
  <div class="header">
    <div class="content-warpper">
      <div class="avater">
        <img :src="seller.avater" alt="" width="64" height="64">
      </div>
    </div>
    <div class="content">
      <div class="title">
        <span class="brand"></span>
        <span class="name">{{ seller.name }}</span>
      </div>
      <div class="descirption">
        {{ seller.descirption}}/{{seller.deliveryTime}}
      </div>
    </div>
    <div class="bulletin-warpper">

    </div>
  </div>
</template>

<script>
export default {
  props: {
    seller: {
      type: Object
    }
  }
}
</script>
