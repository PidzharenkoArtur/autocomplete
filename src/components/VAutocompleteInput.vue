<template>
  <div
  :style="styleAutocompleteInput" 
  class="autocomplete-input"
  >
    <label
    ref="label" 
    class="autocomplete-input__label" 
    for="input">
      {{label}}
    </label>
    <input
      ref="input" 
      id="input" 
      class="autocomplete-input__input" 
      type="text"
      :value="valueInput"
      autocomplete="off"
      @input="onInput"
      @focus="onFocus"
      >
      <div @click="onClick" class="autocomplete-input__arrow-dropdown">
        <svg style="width:24px;height:24px" viewBox="0 0 24 24">
          <path fill="currentColor" d="M7.41,8.58L12,13.17L16.59,8.58L18,10L12,16L6,10L7.41,8.58Z" />
        </svg>
      </div>
      <div @click="clearInput" class="autocomplete-input__clear" >
        <svg style="width:24px;height:24px" viewBox="0 0 24 24">
          <path fill="currentColor" d="M12,20C7.59,20 4,16.41 4,12C4,7.59 7.59,4 12,4C16.41,4 20,7.59 20,12C20,16.41 16.41,20 12,20M12,2C6.47,2 2,6.47 2,12C2,17.53 6.47,22 12,22C17.53,22 22,17.53 22,12C22,6.47 17.53,2 12,2M14.59,8L12,10.59L9.41,8L8,9.41L10.59,12L8,14.59L9.41,16L12,13.41L14.59,16L16,14.59L13.41,12L16,9.41L14.59,8Z" />
        </svg>
      </div>
      <div @click="onBlurClose" class="autocomplete-input__background"></div>
  </div>  
</template>

<script>

export default {
  name: 'VAutocompleteInput',
  props: {
    label: {
      type: String
    },
    width: {
      type: String
    },
    value: {
      type: String   
    }
  },
  data() {
    return {
      isList: null
    }
  },
  computed: {
    styleAutocompleteInput() {
      return {
        width: this.width
      }
    },

    valueInput() {
      return this.value&&this.value.substring(0, this.value.length - 1)
    }
  },
  watch: {
    value(val) {
      this.styleWithActiveValueInput()
      this.onBlur()
      this.$refs.input.value = val
    }
  },
  methods: {
    toggleList() {
      this.isList = !this.isList
    },
    setFocus() {
      this.$refs.input.focus()
    },
    onInput(e) {
      this.$emit('showList', true)  
      this.$emit('input', e.target.value)
    },
    onFocus() {
      this.isList = true
      this.setFocus()
      this.styleWithActiveValueInput()
      this.$emit('showList', this.isList)  
    },
    onBlur() {
      this.isList = false
      this.$emit('showList', this.isList)  
    },
    onBlurClose() {
      this.onBlur()
      this.styleWithActiveValueInput()
    },
    onClick() {
      this.toggleList()
      this.$refs.input.style.height="20px"
      this.isList?this.onFocus():this.onBlur()
      this.styleWithActiveValueInput()
      this.$emit('showList', this.isList)  
    },
    clearInput() {
      this.$refs.input.value = ''
      this.onBlurClose()
    },
    styleInputActive() {
      this.$refs.input.style.height="20px"
      this.$refs.label.style.padding="0"
      this.$refs.label.style.fontSize="13px"
    },
    styleInputNoActive() {
      if (this.isList) {
        this.$refs.label.style.padding="0"
        this.$refs.input.style.height="20px"
        this.$refs.label.style.fontSize="13px"
        return;
      }

      this.$refs.input.style.height="0"
      this.$refs.label.style.fontSize="16px"
    },
    styleWithActiveValueInput() {
      this.$refs.input.value?this.styleInputActive():this.styleInputNoActive()
    }
  }
}
</script>

<style lang="scss" scoped>
.autocomplete-input {
  display: flex;
  justify-content: center;
  flex-direction: column;
  align-items: flex-start;
  background: rgba(0, 0, 0, 0.06);
  height: 60px;
  border-radius: 30px;
  padding-top: 13px;
  padding-left: 30px;
  padding-bottom: 13px;
  box-sizing: border-box;
  position: relative;
  cursor: text;
  &:focus-within &__label,  &:focus-within &__arrow-dropdown  {
    color: #ffab40;
  }
  &:focus-within &__arrow-dropdown {
    transform: rotate(180deg)
  }
  &:focus-within &__input {
    height: 20px;
  }
  &:focus-within &__label{
    height: 20px;
    font-size: 13px;
    padding-top: 0;
    padding-bottom: 0;
  }
  &:hover {
    background: rgba(0, 0, 0, 0.1);
  }
  cursor: text;
  &__label {
    width: 100%;
    padding-top: 20px;
    padding-bottom: 20px;
    text-align: left;
    cursor: text;
    color: rgba(0,0,0,.6);
  }
  &__input {
    display: block;
    height: 0;
    width: 90%;
    outline: none;
    border: none;
    padding: 0;
    background: transparent;
    font-size: 16px;
    color: rgba(0,0,0,.87);
    caret-color: #ffab40;
  }
  &__arrow-dropdown, &__clear{
    position: absolute;
    
    top: 17px;
    cursor: pointer;
  }
  &__arrow-dropdown{
    right: 56px;
    &:hover {
      color: #ffab40;
    }
  }
  &__clear {
    right: 30px;
    &:hover {
      color: #ffab40;
    }
  }
  &__background {
    position: absolute;
    background: transparent;
    width: 100vw;
    height: 100vh;
    left: -100%;
    top: -100%;
    z-index: -1; 
    cursor: default; 
  }
}
</style>