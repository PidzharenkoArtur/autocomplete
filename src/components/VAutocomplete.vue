<template>
  <div class="autocomplete">
    <VAutocompleteInput 
      :width="width"
      :label="label"
      :value="value"
      @input="search=$event"
      @showList="isList=$event"
    />
    
    <VAutocompleteList
      v-if="isList"
      @choose="choose"
      :list="list"
      :search="search"
    />   
  </div>  
</template>

<script>
import VAutocompleteInput from '../components/VAutocompleteInput'
import VAutocompleteList from '../components/VAutocompleteList'

export default {
  name: 'VAutocomplete',
  components: {
    VAutocompleteInput,
    VAutocompleteList
  },
  props: {
    width: {
      type: String
    },
    label: {
      type: String  
    },
    list: {
      type: Array
    },
  },
  data() {
    return {
      search: null,
      value: null,
      isList: null,
      count: 0
    }
  },
  methods: {
    choose(e) {
      this.count++
      this.value=e + this.count
    }
  }
}
</script>

<style lang="scss" scoped>
.autocomplete{
  position: relative;
  display: flex;
  justify-content: center;
}
</style>