<template>
  <div class="home">
    <DaumSearch />
    <KakaoMap />
  </div>
</template>s

<script>
// @ is an alias to /src
import KakaoMap from '@/components/KakaoMap.vue';
import DaumSearch from '@/components/DaumSearch.vue';

export default {
  name: 'HomeView',
  components: {
    KakaoMap,
    DaumSearch,
  }
}
</script>
