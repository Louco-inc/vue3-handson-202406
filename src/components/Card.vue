<script setup>
defineProps({
  id: {
    type: Number,
    default: 0,
    required: false
  },
  name: {
    type: String,
    default: '',
    required: false
  },
  price: {
    type: Number,
    default: 0,
    required: false
  },
  image: {
    type: String,
    default: '',
    required: false
  },
  soldOut: {
    type: Boolean,
    default: false,
    required: false
  }
});

/**
 * 価格を3桁ごとのカンマ付きで返す
 * @param {number} price 価格
 */
const pricePrefix = (price) => {
  return price.toLocaleString()
}

// 「入荷」ボタン、「売り切れ」ボタンを押下した際のイベントを発行するemit
const emit = defineEmits(['stock-item', 'sold-out'])
</script>

<template>
  <div class="thumbnail">
    <img :src="image" alt="">
  </div>
  <div class="description">
    <h2>{{ name }}</h2>
    <slot name="body" />
    <span>¥<span class="price" :class="{ 'is-sold-out': soldOut }">{{ pricePrefix(price) }}</span></span>
    <div v-if="soldOut">
      <span class="sold-out-text">SOLD OUT</span>
      <button type="button" @click="(event) => {
        emit('stock-item', id)
        event.stopPropagation()   // 親要素へのクリックイベント伝播を阻止
      }">入荷</button>
    </div>
    <div v-else>
      <button type="button" @click="(event) => {
        emit('sold-out', id)
        event.stopPropagation()   // 親要素へのクリックイベント伝播を阻止
      }">売り切れ</button>
    </div>
  </div>
</template>

<style>
.thumbnail>img {
  width: 100%;
  height: calc(100%);
  object-fit: cover;
}

.description {
  text-align: left;
  margin-top: 20px;
}

.description>p {
  margin-top: 0px;
  margin-bottom: 0px;
  font-size: 18px;
  line-height: 25px;
}

.description>span {
  display: block;
  margin-top: 10px;
  font-size: 20px;
}

.description>span>.price {
  font-size: 28px;
  font-weight: bold;
}

.sold-out-text {
  color: red;
  font-weight: bold;
}

.is-sold-out {
  color: darkgray;
  text-decoration-line: line-through;
  text-decoration-style: solid;
  text-decoration-color: black;
}
</style>