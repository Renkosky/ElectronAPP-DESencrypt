<template>
    <div class="wrapper">
        <h1 @click='test()'>请输入明文</h1>
        <div class="input-group">
            <span class="input-group-addon"><i class="fa fa-envelope" aria-hidden="true"></i></span>
            <input class="form-control input-item plaintext col-md-4" type="text" placeholder="请输入明文">
            <span class="input-group-addon"><i class="fa fa-key fa-fw"></i></span>
            <input class="form-control input-item key col-md-4" type="password" placeholder="请输入秘钥">
            <div class="butn-enc" @click="grouping">分组加密</div>
        </div>
        
        <!-- <EnEcb :EnEcb="textLen"></EnEcb> -->
        <div class='ecbmode'>
            <div class="ecbkey">
                秘钥：{{ ecbkey }}
            </div>
            <div class="groups">
                <div class="groups-item" v-for="(item,index) in textLen" :key="index">              
                    <h3 class="groups-title">分组 {{ index+1 }}</h3>
                    <div class="groups-content">
                        <div class="plaintextgroup">明文{{ encgroups[index] }}</div>
                        <div class="enc butn-enc" @click='enc(index)'>DES加密</div>
                        <div class="result">
                        </div>
                    </div>           
                </div>
            </div>
              <div class="ecbkey">
                秘钥：{{ ecbkey }}
            </div>
            <div class="groups">
                <div class="groups-item" v-for="(item,index) in textLen" :key="index">              
                    <h3 class="groups-title">分组 {{ index+1 }}</h3>
                    <div class="groups-content">
                        <div class="plaintextgroup">密文 {{ ciphtext[index] }}</div>
                        <div class="enc butn-dec" @click='dec(index)'>DES解密</div>
                        <div class="resultt">
                        </div>
                    </div>           
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
            encgroups:[],
            ecbkey:'',
            result:[],
            keyHex:'',
            ciphtext:[]
        }
    },
    methods: {
        grouping(){
            this.textLen = Math.ceil((document.getElementsByTagName('input')[0].value.length)/8);
            if(this.textLen<1){
                this.textLen = 1;
            }
            var plaintext = document.getElementsByClassName('plaintext')[0].value.split('');
            var plaintxtgroup = [];
            var group=''; 
            for (var i = 1; i <= plaintext.length; i++) {      
                group += plaintext[i-1]
                if ((i%8) == 0 || i == plaintext.length) {
                   plaintxtgroup.push(group);                  
                   group = ''; 
                }                            
            };
            for (var i = 0; i < plaintxtgroup.length; i++) {
                 this.encgroups[i] = plaintxtgroup[i];              
            };
            this.ecbkey = document.getElementsByClassName('key')[0].value;             
           
        },
        enc(id){
            this.result = document.getElementsByClassName('result');
            var Key = document.getElementsByClassName('key')[0].value;
            this.keyHex = CryptoJS.enc.Utf8.parse(Key);  
            var encrypted = CryptoJS.DES.encrypt(this.encgroups[id], this.keyHex, {
            mode: CryptoJS.mode.ECB,
            padding: CryptoJS.pad.Pkcs7
            });
            this.result[id].innerText = encrypted.ciphertext.toString(CryptoJS.enc.Base64);
            this.ciphtext.push(this.result[id].innerText)
        },
        dec(id){
            var resultt =  document.getElementsByClassName('resultt');
            var decrypted = CryptoJS.DES.decrypt({
                ciphertext: CryptoJS.enc.Base64.parse(this.result[id].innerText)
            }, this.keyHex, {
                mode: CryptoJS.mode.ECB,
                padding: CryptoJS.pad.Pkcs7
            });
            resultt[id].innerHTML = decrypted.toString(CryptoJS.enc.Utf8);
            console.log(decrypted.toString(CryptoJS.enc.Utf8));
        }
    }
}
</script>
<style>
    *{
        font-family: Microsoft YaHei,Arial, Helvetica, sans-serif;
    }
    body{
        min-width: 935px;
    }
    h1 {
        margin-bottom: 20px;
        color: #3a585f
    }
    .wrapper{
        margin: 60px 10% 0 10%;
    }
    .input-item{
        margin-right:10px;
    }
    .ecbkey{
        height: 10%;
        margin-right: 40px;
        padding:10px;
        background-color: rgb(251, 251, 253);
    }
    .butn-enc {
        padding: 15px;
        background-color: #337ab7;
        color:white;
        border-radius: 5px;
        max-width: 100px;
        cursor: pointer;
    }
    .butn-dec{
        padding: 15px;
        background-color: #b92c28;
        color:white;
        border-radius: 5px;
        max-width: 100px;
        cursor: pointer;
    }
    .ecbmode{
        display: flex;
        flex-wrap: wrap;
        margin: 60px 20px 20px 20px;
        
    }
    .groups{
        display: flex;
        flex-wrap: wrap;
    }
    .groups div {    
        flex: 0 1 200px;
    }
    .plaintextgroup {
        margin-bottom: 50px;
        font-size:15px;
        color: #3a585f 
    }
    .groups{
        width: 80%;
    }
    .groups-item {
        margin: 0 40px 20px 0px;
        min-width:256px ;
        background-color: rgb(251, 251, 253);
        box-shadow: 1px 1px 10px 0px rgb(220, 220, 220);
        border-radius: 3px;
    }
    .groups-title{
        margin-bottom: 50px;
        padding:20px;
        color: #3a585f;
        border-bottom: 1px solid rgb(173, 216, 230);
        background-color: rgb(245,245,251);
    }
    .groups-content{
        padding:20px;
    }
    .groups-item:hover{
        box-shadow: 2px 3px 0px 2px rgb(220, 220, 220);
    }
    .result{
        height: 25%;
        width: 100%;
        margin-top: 50px;
        color: #3a585f;
        font-size:15px;
    }
      .resultt{
        height: 25%;
        width: 100%;
        margin-top: 50px;
        color: #3a585f;
        font-size:15px;
    }
  
</style>
