<template>
<div>
<img :src="imgCode" >

</div>
</template>

<script>
import axios from "axios";

export default {
  name: "image",
  data(){
    return{
      imgCode: '',
      url: '',

    }
  },
  methods:{
    getImgCode () {
      axios({

        method: 'get',
        url: '/img-response',
        responseType: 'arraybuffer' // 最为关键
      })
          .then((res)=> {
            return (
                'data:image/jpeg;base64,'+
                    btoa(new Uint8Array(res).reduce((data, byte)=> data+String.fromCharCode(byte), ''))
            )

          }).then((data)=>{
            this.imgCode = data
          }

      )
    },
    arrayBufferToBase64 (buffer) {
      var binary = ''
      var bytes = new Uint8Array(buffer)
      var len = bytes.byteLength
      for (var i = 0; i < len; i++) {
        binary += String.fromCharCode(bytes[i])
      }
      return window.btoa(binary)
    },
    markFace(){
      const _this = this
      _this.$axios.post("/extractMaxFace").then(res =>{
        // console.log(res)
        _this.imgCode = res
        // console.log(_this.pageSize)
      })
    }

  },created(){
    // console.log("created")
    this.markFace()
  }
}
</script>

<style scoped>

</style>