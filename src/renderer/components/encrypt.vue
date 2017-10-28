<template>
    <div>
        <input type="text" class="plaintext"> 
        <div @click="getLen">输入明文</div>
        <!-- <EnEcb :EnEcb="textLen"></EnEcb> -->
        <div class='ecbmode'>
        <div v-for="(item,index) in textLen" :key="index">
            <div class="plaintextgroup">{{ encgroups[index] }}</div>
            <div class="enc" @click='enc(index)'>DES加密</div>
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
            textLen:0,
            encgroups:[]
        }
    },
    methods: {
        getLen(){
            this.textLen = Math.ceil((document.getElementsByTagName('input')[0].value.length)/8);
            if(this.textLen<1){
                this.textLen = 1;
            }
            var plaintext = document.getElementsByClassName('plaintext')[0].value.split('');
            var plaintxtgroup = [];
            var group=''; 
            for (var i = 0; i < plaintext.length; i++) {      
                group += plaintext[i]
                if ((7/i) == 1 || i == plaintext.length-1) {
                   plaintxtgroup.push(group);                  
                   group = ''; 
                }                            
            };
            var encgroup = document.getElementsByClassName('plaintextgroup')
            for (var i = 0; i < plaintxtgroup.length; i++) {
                 this.encgroups[i] = plaintxtgroup[i];              
            }            
           
        },
        enc(id){
            var result = document.getElementsByClassName('result')[id];
            var keyHex = CryptoJS.enc.Utf8.parse('01234567');  
            var encrypted = CryptoJS.DES.encrypt(this.encgroups[id], keyHex, {
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
