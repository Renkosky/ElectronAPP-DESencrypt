<template>
    <div>
        <input type="text"> 
        <div @click="getLen">输入明文</div>
        <!-- <EnEcb :EnEcb="textLen"></EnEcb> -->
        <div class='ecbmode'>
        <div v-for="item in textLen" :key="item.id">
            <div><input type="text" class="plaintext"></div>
            <div class="enc" @click='enc()'>DES加密</div>
            <div class="result">
            </div>
        </div>
        </div>
    </div> 
</template>
<script>
    var DES = require('crypto-js/tripledes')
    var CryptoJS = require('crypto-js')
    var EnEcb = require('./EncEcb')

    export default {
    name: 'encrypt',
    components: {
        EnEcb
    },
    data(){
        return {
            textLen:0
        }
    },
    methods: {
        getLen(){
            this.textLen = Math.ceil((document.getElementsByTagName('input')[0].value.length)/8);
            if(this.textLen<1){
                this.textLen = 1;
            }        
            return this.textLen;
        },
        enc(){
            var encrypt = document.getElementsByClassName('enc')[0];
            var result = document.getElementsByClassName('result')[0];
            var plaintext = document.getElementsByClassName('plaintext')[0].value;
            var keyHex = CryptoJS.enc.Utf8.parse('01234567');  
            var encrypted = CryptoJS.DES.encrypt(plaintext, keyHex, {
            mode: CryptoJS.mode.ECB,
            padding: CryptoJS.pad.Pkcs7
            });
            result.innerHTML = encrypted.ciphertext.toString(CryptoJS.enc.Base64);
      }
    }
}
</script>
<style>
.ecbmode{
    display: flex
}
.ecbmode div {
    flex: 1 0 auto;
}
</style>
