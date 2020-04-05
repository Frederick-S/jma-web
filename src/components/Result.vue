<template>
  <section>
  </section>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator'
import axios from '../axios'
import bus from '../bus'
import spinner from '../spinner'
import toast from '../toast'

@Component
export default class Result extends Vue {
  analyse(text: string) {
    spinner.open()

    axios.post('/tokenize', { text })
      .then((data) => {
        console.log(data)
      })
      .catch((error) => [
        toast.danger(error)
      ])
      .finally(() => {
        spinner.close()
      })
  }

  mounted() {
    bus.$on('analyse', this.analyse)
  }
}
</script>
