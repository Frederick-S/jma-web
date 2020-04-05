<template>
  <section>
    <b-field position="is-centered">
      <b-input placeholder="Tokenize" type="search" icon="magnify" @keyup.enter.native="search" v-model="text"></b-input>
    </b-field>
  </section>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator'
import bus from '../bus'

@Component
export default class Search extends Vue {
  private text = ''

  search() {
    if (this.text) {
      bus.$emit('analyse', this.text)
    }
  }
}
</script>
