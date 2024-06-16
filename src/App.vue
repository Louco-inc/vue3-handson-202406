<script setup>
import { ref, computed } from 'vue'
import Card from './components/Card.vue'
import CardBody from './components/CardBody.vue'

const items = ref([
  {
    id: 1,
    name: 'アボカドソースバケット',
    description: '刻んだ野菜をアボカドと混ぜ、優しい味のソースに。こんがり焼いたバゲットとお召し上がりください。',
    price: 320,
    image: '/images/item1.jpg',
    soldOut: false,
    selected: false,
    link: 'https://handson.vuejs-jp.org/'
  },
  {
    id: 2,
    name: 'あの日夢見たホットケーキ',
    description:
      '子供のころに食べたかった、あのホットケーキを再現しました。素朴でどこか懐かしい味をどうぞ。',
    price: 1180,
    image: '/images/item2.jpg',
    soldOut: true,
    selected: false
  },
  {
    id: 3,
    name: 'HOP WTR',
    description:
      'ロサンゼルス生まれのスパークリングウォーター。ノンカロリー、ノンアルコールの新感覚飲料です。',
    price: 320,
    image: '/images/item3.jpg',
    soldOut: true,
    selected: false
  },
  {
    id: 4,
    name: 'チーズフレンチフライ',
    description:
      'イタリア産チーズをたっぷりかけたアツアツのフレンチフライ。みんな大好きな一品です。',
    price: 670,
    image: '/images/item4.jpg',
    soldOut: false,
    selected: false
  }
])

/**
 * 在庫のある商品数を返す
 */
const stockQuantity = computed(() => {
  return items.value.filter((item) => !item.soldOut).length
})

/**
 * 商品の在庫状況を変更する
 * @param {Number} id 商品のid
 */
const stockItem = (id) => {
  const targetItem = items.value.find((item) => item.id === id)
  targetItem.soldOut = false
}

/**
 * 商品を売り切れ状態にする
 * @param {Number} id 商品のid 
 */
const changeSoldOut = (id) => {
  const soldOutItem = items.value.find((item) => item.id === id)
  soldOutItem.soldOut = true
}
</script>

<template>
  <header class="header">
    <img src="/images/logo.svg" alt="">
    <h1>Vue.js ハンズオン</h1>
  </header>
  <div>販売商品数：{{ stockQuantity }}</div>
  <main class="main">
    <template v-for="item in items" :key="item.id">
      <div class="item" :class="{ 'selected-item': item.selected }" @click="item.selected = !item.selected"
        @keyup.enter="item.selected = !item.selected" tabindex="0">
        <Card :id="item.id" :image="item.image" :name="item.name" :price="item.price" :soldOut="item.soldOut"
          @stock-item="stockItem" @sold-out="changeSoldOut">
          <template #body>
            <CardBody 
              :description="item.description"
              :link="item.link"
            />
          </template>
        </Card>
      </div>
    </template>
  </main>
</template>

<style>
body {
  font-family: sans-serif;
  margin: 0;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

#app {
  width: 90%;
  margin: 0 5%;
  color: #242424;
}

.header {
  display: flex;
  align-content: center;
  align-items: center;
  margin: 40px 0;
}

.header>img {
  width: 100px;
  height: 100px;
  margin-right: 20px;
}

.header>h1 {
  font-size: 80px;
  font-weight: bold;
  line-height: 80px;
  margin: 0;
}

.main {
  display: grid;
  grid-template-columns: 3fr 3fr 3fr 3fr;
  column-gap: 24px;
  row-gap: 24px;
}

.item {
  padding: 10px;
  cursor: pointer;
}

.item:hover {
  transition: 0.2s transform ease-out;
  transform: scale(1.05);
}

.selected-item {
  background-color: #e3f2fd;
}
</style>
