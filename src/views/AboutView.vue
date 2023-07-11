<template>
  <div class="about">
    <h1>This is an about page</h1>
    <KakaoMap />
  </div>
</template>

<script>
import KakaoMap from '@/components/KakaoMap.vue';

export default {
  name: 'AboutView',
  components: {
    KakaoMap,
  }
}

</script>