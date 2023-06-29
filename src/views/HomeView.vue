<template>
  <div class="home">
    <KakaoMap />
  </div>
</template>s

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'
import KakaoMap from '@/components/KakaoMap.vue';

export default {
  name: 'HomeView',
  components: {
    HelloWorld,
    KakaoMap
  }
}
</script>
