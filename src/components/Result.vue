<template>
  <section>
    <b-table :data="data" :columns="columns" :sticky-header="true" :height="400" class="token-list" v-if="data.length > 0"></b-table>
  </section>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator'
import axios from '../axios'
import bus from '../bus'
import spinner from '../spinner'
import toast from '../toast'
import Token from '../token'

interface Data {
  surface: string;

  partOfSpeech: string;

  baseForm: string;

  reading: string;

  pronunciation: string;
}

@Component
export default class Result extends Vue {
  private columns = [
    {
      field: 'surface',
      label: 'Surface form'
    },
    {
      field: 'partOfSpeech',
      label: 'Part-of-Speech'
    },
    {
      field: 'baseForm',
      label: 'Base form'
    },
    {
      field: 'reading',
      label: 'Reading'
    },
    {
      field: 'pronunciation',
      label: 'Pronunciation'
    }
  ]

  private data: Data[] = []

  analyse(text: string) {
    spinner.open()

    axios.post('/tokenize', { text })
      .then((data) => {
        const tokens: Token[] = data.data

        this.data = tokens.map(token => {
          return {
            surface: token.surface,
            partOfSpeech: [token.partOfSpeechLevel1, token.partOfSpeechLevel2, token.partOfSpeechLevel3, token.partOfSpeechLevel4].join('，'),
            baseForm: token.baseForm,
            reading: token.reading,
            pronunciation: token.pronunciation
          }
        })
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
