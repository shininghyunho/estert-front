<template>
  <div class="home">
    <div class="search">
      <DaumSearch />
    </div>
  </div>
</template>s

<script>
// @ is an alias to /src
import DaumSearch from '@/components/DaumSearch.vue';

export default {
  name: 'HomeView',
  components: {
    DaumSearch,
  }
}
</script>