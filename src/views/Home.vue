<template>
  <div class="column is-half">
    <Search></Search>
    <Result></Result>
  </div>
</template>

<script>
import Search from '@/components/Search.vue'
import Result from '@/components/Result.vue'

export default {
  name: 'Home',
  components: {
    Search,
    Result
  }
}
</script>
