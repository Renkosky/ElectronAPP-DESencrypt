<template>
    <div class="wrapper">
        <h1>请输入明文</h1>
        <div class="input-group">
            <span class="input-group-addon"><i class="fa fa-envelope" aria-hidden="true"></i></span>
            <input class="form-control input-item plaintext col-md-4" type="text" placeholder="请输入明文">
            <span class="input-group-addon"><i class="fa fa-key fa-fw"></i></span>
            <input class="form-control input-item key col-md-4" type="password" placeholder="请输入秘钥">
            <div class="butn-enc" @click="getLen">分组加密</div>
        </div>
        
        <!-- <EnEcb :EnEcb="textLen"></EnEcb> -->
        <div class='ecbmode'>
            <div class="groups" v-for="(item,index) in textLen" :key="index">
                <h3 class="groups-title">分组 {{ index+1 }}</h3>
                <div class="plaintextgroup">{{ encgroups[index] }}</div>
                <div class="enc butn-enc" @click='enc(index)'>DES加密</div>
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
            for (var i = 1; i <= plaintext.length; i++) {      
                group += plaintext[i-1]
                if ((i%7) == 0 || i == plaintext.length) {
                   plaintxtgroup.push(group);                  
                   group = ''; 
                }                            
            };
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
    *{
        font-family: Microsoft YaHei,Arial, Helvetica, sans-serif;
    }
    h1 {
        margin-bottom: 20px;
        color: #3a585f
    }
    .wrapper{
        margin: 60px 20% 0 20%;
    }
    .input-item{
        margin-right:10px;
    }
    .butn-enc{
        padding: 15px;
        background-color: #337ab7;
        color:white;
        border-radius: 5px;
        max-width: 100px;
        cursor: pointer;
    }
    .ecbmode{
        display: flex;
        margin: 60px 20px 20px 20px; 
    }
    .ecbmode div {
        flex: 1 0 auto;
    }
    .plaintextgroup {
        margin-bottom: 50px;
        font-size:25px;
        color: #3a585f 
    }
    .groups-title{
        margin-bottom: 50px;
        color: #3a585f 
    }
    .groups {
        margin-right: 40px;
    }
</style>
