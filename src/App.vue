<template lang="pug">
.coffee__shop 
  .coffee__shop__title 咖啡訂單系統
  .coffee__shop__type(v-for="(item, i) in coffeeList" :class="{background__color : i%2!==0, font__weight : i===0}")
    .coffee__shop__type__name {{item.name}}
    .coffee__shop__type__price {{item.price}}
    .coffee__shop__type__size {{item.size}}
    .coffee__shop__type__notes {{item.notes}}
    .coffee__shop__type__btn
      .add__btn(v-if="i === 0" @click="addCoffeeData()") 新增
      .edit__btn(v-if="i !== 0" @click="editCoffeeData(i)") 編輯
      .delete__btn(v-if="i !== 0" @click="deleteCoffeeData(i)") 刪除
  .pop(v-if="mask")
    .pop__mask
      .pop__group
        .pop__title {{type}}
        .pop__text 品項名稱
        input(v-model="tempName")
        .pop__text 價格
        input(v-model="tempPrice")
        .pop__text 尺寸(L/M/S)
        input(v-model="tempSize")
        .pop__text 備註
        input(v-model="tempNotes")
        .pop__btn__group
          .pop__btn(v-if="type === '新增'" @click="enterAddCoffeeData()") 確定
          .pop__btn(v-else-if="type === '編輯'" @click="enterEditCoffeeData()") 確定
          .pop__btn(@click.self="dataInit()") 取消
</template>

<script>

export default {
  name: 'coffee__shop',
  data() {
    return {
      coffeeList: [
        { name: '品項名稱', price: '價格', size: 'L/M/S', notes: '備註' },
        { name: '美式咖啡', price: 45, size: 'L', notes: '這是備註字很長很多很長很多很長很多很長很多很長很多很長很多很長很多很長很多很長很多很長很多很長很多' },
        { name: '美式咖啡', price: 35, size: 'M', notes: '' },
        { name: '美式咖啡', price: 25, size: 'S', notes: '' },
        { name: '拿鐵', price: 55, size: 'L', notes: '' },
        { name: '拿鐵', price: 45, size: 'M', notes: '' },
        { name: '拿鐵', price: 35, size: 'S', notes: '' },
      ],
      mask: false,
      type: '新增',
      editId: 0,
      tempName: '',
      tempPrice: '',
      tempSize: '',
      tempNotes: '',
    }
  },

  watch: {
    'mask' () {
      this.mask === true ? document.body.style.overflow = 'hidden' : document.body.style.overflow = ''
    },
  },

  methods: {
    deleteCoffeeData(id) {
      this.coffeeList.splice(id,1)
    },
    addCoffeeData() {
      this.type = '新增'
      this.mask = true
    },
    enterAddCoffeeData() {
      if(!this.tempName && !this.tempPrice && !this.tempSize && !this.tempNotes) {
        this.mask = false 
        return
      }
      this.coffeeList.push({
        name: this.tempName,
        price: this.tempPrice,
        size: this.tempSize,
        notes: this.tempNotes,
      })
      this.dataInit()
    },
    editCoffeeData(i) {
      this.type = '編輯'
      this.editId = i
      this.tempName = this.coffeeList[i].name
      this.tempPrice = this.coffeeList[i].price
      this.tempSize = this.coffeeList[i].size
      this.tempNotes = this.coffeeList[i].notes
      this.mask = true
    },
    enterEditCoffeeData() {
      this.coffeeList[this.editId].name = this.tempName
      this.coffeeList[this.editId].price = this.tempPrice
      this.coffeeList[this.editId].size = this.tempSize
      this.coffeeList[this.editId].notes = this.tempNotes
      this.mask = false
    },
    dataInit() {
      this.tempName = ''
      this.tempPrice= ''
      this.tempSize= ''
      this.tempNotes= ''
      this.mask = false
    }
  },
}
</script>

<style lang="scss" scoped>
  @import './style.scss';
</style>
