<template>
  <div id="app">
    <b-form-group
      label="태그"
      label-for="basicStatic"
      :label-cols="2"
      :horizontal="true">
      <b-form-input-tag class="form-control" 
        v-model="tags"
        :limit="10" 
        :placeholder="'최대 10개까지 등록 가능합니다.'"
        :separators="','"></b-form-input-tag>
    </b-form-group>
  </div>
</template>

<script>
import bFormInputTag from './components/bFormInputTag'

export default {
  name: 'app',
  data() {
    return {
      tags: 'tag1,tag2,tag3,tag4,tag5'
    }
  },
  components: {
    bFormInputTag
  }
}
</script>

<style>
/* #app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
} */
</style>
