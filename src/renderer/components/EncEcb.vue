<template>
  <div>
    <div v-for="item in textLen" :key="item.id">
        <input type="text">
        <div class="enc" @click='enc()'>DES加密</div>
        <div class="result"></div>
    </div>
  </div>
</template>
<script>
export default {
  name:'EncEcb',
  props:{
      textLen:Number
  },
  methods: {
      enc(){
        var textLen = document.getElementsByTagName('input')[0].value.length;
        var encrypt = document.getElementsByClassName('enc')[0];
        var result = document.getElementsByClassName('result')[0];
        var keyHex = CryptoJS.enc.Utf8.parse('01234567');  
        var encrypted = CryptoJS.DES.encrypt(text, keyHex, {
          mode: CryptoJS.mode.ECB,
          padding: CryptoJS.pad.Pkcs7
    });
        result.innerHTML = encrypted.ciphertext.toString(CryptoJS.enc.Base64);
      }
  }
}
</script>
<style scoped>

</style>
