<template>
    <div>
        <button @click="search">주소 검색</button>
        <div>{{postCode}}</div>
        <div>{{address}}</div>
        <div>{{detailAddress}}</div>
        <div>{{extraAddr}}</div>
    </div>
    <div id="wrap" style="">
        <button @click="fold">주소창 닫기</button>
    </div>
</template>

<script>
export default {
    name: "DaumSearch",
    data() {
        return {
            postCode: "",
            address: "",
            detailAddress: "",
            extraAddr: "",
        }
    },
    mounted() {
        // script load
        if(!window.daum) {
            this.loadScript();
        }
    },
    methods: {
        loadScript() {
            const script = document.createElement("script");
            script.src = "//t1.daumcdn.net/mapjsapi/bundle/postcode/prod/postcode.v2.js";
            console.log(script+",script is loaded")
            document.head.appendChild(script);
        },
        search() {
            // 우편번호 찾기 찾기 화면을 넣을 element
            const element_wrap = document.getElementById("wrap");

            // 현재 scroll 위치를 저장해둔다.
            const currentScroll = Math.max(document.body.scrollTop, document.documentElement.scrollTop);
            // 우편 코드 찾기 화면을 띄운다.
            new window.daum.Postcode({
                // oncomplete
                oncomplete: (data) => {
                    // 검색결과 항목을 클릭했을때 실행할 코드를 작성하는 부분.

                    // 각 주소의 노출 규칙에 따라 주소를 조합한다.
                    // 내려오는 변수가 값이 없는 경우엔 공백('')값을 가지므로, 이를 참고하여 분기한다.
                    let addr = ""; // 주소 변수
                    let extraAddr = ""; // 참고항목 변수

                    //사용자가 선택한 주소 타입에 따라 해당 주소 값을 가져온다.
                    if(data.userSelectedType === "R") { // 사용자가 도로명 주소를 선택했을 경우
                        addr = data.roadAddress;
                    } else { // 사용자가 지번 주소를 선택했을 경우(J)
                        addr = data.jibunAddress;
                    }

                    // 사용자가 선택한 주소가 도로명 타입일때 참고항목을 조합한다.
                    if(data.userSelectedType === "R") {
                        // 법정동명이 있을 경우 추가한다. (법정리는 제외)
                        // 법정동의 경우 마지막 문자가 "동/로/가"로 끝난다.
                        if(data.bname !== "" && /[동|로|가]$/g.test(data.bname)) {
                            extraAddr += data.bname;
                        }
                        // 건물명이 있고, 공동주택일 경우 추가한다.
                        if(data.buildingName !== "" && data.apartment === "Y") {
                            extraAddr += (extraAddr !== "" ? ", " + data.buildingName : data.buildingName);
                        }
                        // 건물명이 있고, 공동주택이 아닐 경우 추가한다.
                        if(data.buildingName !== "" && data.apartment === "N") {
                            extraAddr += (extraAddr !== "" ? ", " + data.buildingName : data.buildingName);
                        }
                        // 표시할 참고항목이 있을 경우, 괄호까지 추가한 최종 주소를 만든다.
                        if(extraAddr !== "") {
                            extraAddr = " (" + extraAddr + ")";
                        }
                        // 조합된 참고항목을 해당 필드에 넣는다.
                        this.extraAddr = extraAddr;
                    } else {
                        this.extraAddr = "";
                    }

                    // 우편번호와 주소 정보를 해당 필드에 넣는다.
                    this.postcode = data.zonecode;
                    this.address = addr;
                    // 커서를 상세주소 필드로 이동한다.
                    // this.detailAddress.focus();
                    this.detailAddress = "";

                    // iframe을 넣은 element를 안보이게 한다.
                    element_wrap.style.display = "none";

                    // 우편번호 찾기 화면이 보이기 이전으로 scroll 위치를 되돌린다.
                    document.body.scrollTop = currentScroll;
                },
                // onresize, 우편번호 찾기 화면 크기가 변경되었을때 실행되는 함수
                onresize: (size) => {
                    element_wrap.style.height = size.height + "px";
                },
                width: "100%",
                height: "100%",
            }).embed(element_wrap);

            // iframe을 넣은 element를 보이게 한다.
            element_wrap.style.display = "block";
        },
        fold() {
            const element_wrap = document.getElementById("wrap");
            // iframe을 넣은 element를 안보이게 한다.
            element_wrap.style.display = "none";
        }
    }
}

</script>

<style scoped>
#wrap {
    display: none;
    border: 1px solid;
    width: 500px;
    height: 300px;
    margin: 5px 0;
    position: relative;
}
</style>