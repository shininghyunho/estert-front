<template>
    <div>
        <div id="map"></div>
    </div>
</template>

<script>
const kakaoKey = "23ffa58a55d387dae088dff88a0a0234"

export default {
    name: "KakaoMap",
    data() {
        return {
            map: null,
        }
    },
    mounted() {
        if(window.kakao && window.kakao.maps) {
            // 카카오맵 API가 로드되어 있으면 바로 지도를 생성한다.
            this.loadMap();
        } else {
            // 카카오맵 API가 로드되어 있지 않으면, API 로드가 완료될 때까지 기다린다.
            this.loadScript();
        }
    },
    methods: {
        loadScript() {
            const script = document.createElement("script");
            script.src = `//dapi.kakao.com/v2/maps/sdk.js?autoload=false&appkey=${kakaoKey}`
            script.onload = () => {
                // 카카오맵 API가 로드되면, 지도를 생성한다.
                window.kakao.maps.load(() => {
                    this.loadMap();
                });
            }
            // script 태그를 <head> 태그에 추가한다.
            document.head.appendChild(script);
        },
        loadMap() {
            // 지도를 표시할 div id
            const container = document.getElementById("map");
            // 지도 생성시 필요한 옵션
            const options = {
                center: new window.kakao.maps.LatLng(33.450701, 126.570667),
                level: 3,
            };
            this.map = new window.kakao.maps.Map(container, options);
            // 마커를 표시한다.
            this.loadMarker();
        },
        loadMarker() {
            const markerPosition = new window.kakao.maps.LatLng(33.450701, 126.570667);
            const marker = new window.kakao.maps.Marker({
                position: markerPosition,
            });
            marker.setMap(this.map);
        }
    }
}
</script>

<style scoped>
#map {
    width: 500px;
    height: 400px;
    /* 중앙 정렬 */
    margin: 0 auto;
}
</style>