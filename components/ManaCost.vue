<template>
  <ul class="mana-cost">
    <li v-for="(manaElement, index) in manaCostElements" v-bind:key="index">
      <i v-if="manaElement.type === 'icon'">{{ manaElement.text }}</i>
      <img v-if="manaElement.type === 'img'" :src="manaElement.url" />
    </li>
  </ul>
</template>

<script>
  export default {
    props: {
      manaCost: {
        type: String,
        required: false
      }
    },
    data() {
      return {
        manaCostElements: []
      }
    },
    created () {
      if (!this.manaCost || this.manaCost == undefined) {
        return
      }
      var manaElements = this.manaCost.slice(1, -1)
      var manaElementsArray = manaElements.split('}{')
      var manaCostElements = []

      manaElementsArray.forEach(function(manaItem, index) {
        // Cas des couleurs : W, B, G, R, U
        // White, Black, Green, Red, Blue
        if (isNaN(parseInt(manaItem))) {
          manaCostElements.push({
            type: 'img',
            url: '/images/' + manaItem + '.png'
          })
        } else { // Cas des manas incolores
          manaCostElements.push({
            type: 'icon',
            text: manaItem
          })
        }
      })
      this.manaCostElements = manaCostElements
    }
  }
</script>

<style scoped>
ul {
  display: flex;
  list-style-type: none;
  padding: 0;
  margin: 10px 0;
}
li {
  list-style: none;
  margin: 0 4px;
  display: flex;
  align-items: center
}
i {
  height: 15px;
  width: 15px;
  border-radius: 50%;
  color: #000;
  background: #CCC;
  display: inline-block;
  font-style: normal;
  text-align: center;
  font-size: 14px;
  line-height: 17px;
}
</style>
