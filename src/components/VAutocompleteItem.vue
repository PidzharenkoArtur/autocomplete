<template>
  <div
  ref="item"
  @click="chooseItem()"
  class="autocomplete-item"
  :style="styleItem"
  >
    {{item}}
  </div>  
</template>

<script>

export default {
  name: 'VAutocompleteItem',
  props: {
    item: {
      type: String
    },
    index: {
      type: Number  
    },
    itemIndex: {
      type: Number  
    },
    scrollDownIndex: {
      type: Number
    }
  },
  computed: {
    styleItem() {
      if (this.index !== this.itemIndex) return

      return {
        background:'rgba(0, 0, 0, 0.3)'
      }
    }  
  },
  watch: {
    scrollDownIndex(index) {
      if (index === this.index) {
        this.$emit('moveScroll', {
          indexCurrent: this.index,
          offsetTop:this.$refs.item.offsetTop
        })  
      }
    } 
  },
  methods: {
    chooseItem() {
      this.$emit('choose', this.item)  
    }
  }
}
</script>

<style lang="scss" scoped>
.autocomplete-item {
  width: 100%;
  display: flex;
  padding-left: 20px;
  padding-top: 10px;
  padding-bottom: 10px;
  box-sizing: border-box;
  cursor: pointer;
  &:hover {
    background: rgba(0, 0, 0, 0.1);
  }
}
</style>