<template>
  <div
  ref="list"
  class="autocomplete-list"
  >
    <VAutocompleteItem
    @choose="$emit('choose', $event)"
    @moveScroll="moveScroll" 
    v-for="(item, index) in searchList"
    :key="index"
    :item="item"
    :index="index"
    :itemIndex="itemIndex"
    :scrollDownIndex="scrollDownIndex"
    /> 
    <div class="autocomplete-list__notfind" v-if="isfilterList">Данных нет по такому запросу</div> 
  </div>  
</template>

<script>
import VAutocompleteItem from '../components/VAutocompleteItem'

export default {
  name: 'VAutocompleteList',
  props: {
    list: {
      type: Array
    },
    search: {
      type: String
    },
  },
  data() {
    return {
      itemIndex: -1,
      KeyupDown: 40,
      KeyupTop: 38,
      KeyupEnter: 13,
      scrollDown: 0,
      scrollDownIndex: 0
    }
  },
  components: {
    VAutocompleteItem
  },
  computed: {
    filterList() {
      return this.list.filter((item)=>{
        return item.indexOf(this.search) != -1
      })
    },
    searchList() {
      if (this.isfilterList && !this.search) {
        return this.list
      }

      return this.filterList
    },

    filterListLength() {
      return this.searchList.length-1
    },

    isfilterList() {
      return !this.filterList.length
    },
  },
  methods: {
    moveScroll(data) {
      this.$refs.list.scrollTop = data.offsetTop-20
    },
    doScrollDown() {
      this.scrollDown++

      if (this.scrollDown > 7) {
        this.scrollDownIndex = this.itemIndex
        this.scrollDown = 0
      }
    },
    onKeyupDown() {
      this.itemIndex++
      if (this.itemIndex > this.filterListLength) {
        this.itemIndex = 0 
      }
      
      this.doScrollDown()
    },
    onKeyupTop() {
      this.itemIndex--
      if (this.itemIndex < 0) {
        this.itemIndex = this.filterListLength 
      }
    },
    onKeyupEnter() {
      this.$emit('choose', this.searchList[this.itemIndex])
    }
  },
  created() {
    document.onkeydown = evt => {
      evt = evt || window.event;
      if (evt.keyCode == this.KeyupDown) {
        this.onKeyupDown()
      }
      if (evt.keyCode == this.KeyupTop) {
        this.onKeyupTop()
      }
      if (evt.keyCode == this.KeyupEnter) {
        this.onKeyupEnter()
      }
    };
  }
}
</script>

<style lang="scss" scoped>
.autocomplete-list {
  display: flex;
  width: 100%;
  position: absolute;
  flex-direction: column;
  align-items: flex-start;
  background: rgba(0, 0, 0, 0.06);
  border-radius: 5px;
  padding-top: 13px;
  box-sizing: border-box;
  gap: 10px;
  top: 60px;
  overflow-y: auto;
  max-height: 400px;
  &__notfind {
    padding: 20px;
    padding-top: 10px;
  }
}
</style>