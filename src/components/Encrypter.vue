<template>
  <form name="encform" onsubmit="return false;">
    <div class="main">

      <label for="exampleFormControlTextarea1"
             class="form-label">
        Исходные данные</label>
      <textarea name="box1"
                v-model="box1"
                class="form-control"
                id="exampleFormControlTextarea1"
                placeholder="Введите текст для зашифровки/расшифровки..."
                rows="3"></textarea>

      <div class="cryptButtons">
        <button class="btn btn-primary"
                @click="sha256()">
          SHA256
        </button>

        <button class="btn btn-primary"
                @click="VernamCrypt()">
          Шифр Вернама
        </button>

        <button class="btn btn-primary"
                @click="CezarCrypt()">
          Шифр Цезаря
        </button>
      </div>


      <div class="cryptButtonsMain">
        <button class="btn btn-primary"
                @click="AesCrypt()">
          AES
        </button>
        <button class="btn btn-primary"
                @click="DesCrypt()">
          DES
        </button>
        <button class="btn btn-primary"
                @click="TripleDESCrypt()">
          Triple DES
        </button>
      </div>


      <label for="exampleFormControlTextarea2"
             class="form-label">
        Результат программы</label>
      <textarea name="box2"
                class="form-control"
                v-model="box2"
                id="exampleFormControlTextarea2"
                rows="3"
                placeholder="Данные после шифрации/дешифрации"></textarea>


      <div class="unCryptButtons">
        <button class="btn btn-primary"
                @click="CezarunUncrypt()">
          Расшифровать шифр Цезаря
        </button>
        <button class="btn btn-primary"
                @click="AesUncrypt()">
          Расшифровать AES
        </button>
        <button class="btn btn-primary"
                @click="DesUncrypt()">
          Расшифровать DES
        </button>
        <button class="btn btn-primary"
                @click="TripleDESUncrypt()">
          Расшифровать Triple DES
        </button>
      </div>


    </div>
  </form>
</template>

<script>
import sha256 from 'crypto-js/sha256';
var CryptoJS = require("crypto-js");

export default {
  name: 'EncrypterFunctions',
  data() {
    return {
      box1: null,
      box2: null
    }
  },
  props: {
    msg: String
  },
  methods: {
         //worked
       CezarCrypt() {
         let theText = this.box1
         let output  = new String;
         let Temp    = new Array();
         let Temp2   = new Array();
         let TextSize = theText.length;
        for (let i = 0; i < TextSize; i++) {
          let rnd = Math.round(Math.random() * 122) + 68;
          Temp[i] = theText.charCodeAt(i) + rnd;
          Temp2[i] = rnd;
        }
        for (let i = 0; i < TextSize; i++) {
          output += String.fromCharCode(Temp[i], Temp2[i]);
        }
        this.box2 = output;
      },

        //worked
       CezarunUncrypt() {
        let theText = this.box2
        let output  = new String;
        let Temp    = new Array();
        let Temp2   = new Array();
        let TextSize = theText.length;
        for (let i = 0; i < TextSize; i++) {
          Temp[i] = theText.charCodeAt(i);
          Temp2[i] = theText.charCodeAt(i + 1);
        }
        for (let i = 0; i < TextSize; i = i+2) {
          output += String.fromCharCode(Temp[i] - Temp2[i]);
        }
        this.box2 = output;
      },


      //worked
       VernamCrypt() {
         var theText = this.box1
        // генератор случайных чисел в заданном диапазоне
        function getRandomInt(min, max) {
          return Math.floor(Math.random() * (max - min)) + min;
        }
        var input, output, key;
        var inp, k;
        input = theText

        key = prompt("Введите ключ и запишите его - он потребуется для расшифровки сообщения");

        // если длина ключа меньше длины сообщения — говорим пользователю и генерируем свой ключ
        if ((key.length) < (input.length)) {
          alert("Ключ короче сообщения, это небезопасно. Скопируйте новый сгенерированный ключ из консоли браузера.");
          key = "";
          // генерируем новый ключ такой же длины, как и сообщение
          for (var i = 0; i < input.length; i++) {
            key += String.fromCharCode(getRandomInt(0, 66535));
          }
          console.log("Скопируйте новый ключ ↓");
          console.log(key);
        }
        output = "";
        for (i = 0; i < input.length; i++) {
          inp = input.charCodeAt(i);
          k = key.charCodeAt(i);
          output += String.fromCharCode(inp ^ k);
        }
        this.box2 = output;
      },

    //worked
      AesCrypt() {
        this.box2 = CryptoJS.AES.encrypt(this.box1, 'secret key').toString()
      },

     //worked
      AesUncrypt() {
        var bytes  = CryptoJS.AES.decrypt(this.box2, 'secret key');
        this.box2 = bytes.toString(CryptoJS.enc.Utf8);
      },

      //worked but useless :P TODO: do something with this donkey shit
      sha256() {
         this.box2 = sha256(this.box1)
      },

      //worked
      DesCrypt() {
         this.box2 = CryptoJS.DES.encrypt(this.box1, 'secret key').toString();
      },

      //worked
      DesUncrypt() {
        var bytes = CryptoJS.DES.decrypt(this.box2, 'secret key');
        this.box2 = bytes.toString(CryptoJS.enc.Utf8);
      },

      //worked
      TripleDESCrypt() {
        this.box2 = CryptoJS.TripleDES.encrypt(this.box1, "Secret Passphrase").toString();
      },

      //worked
      TripleDESUncrypt() {
        var bytes = CryptoJS.TripleDES.decrypt(this.box2, "Secret Passphrase");
        this.box2 = bytes.toString(CryptoJS.enc.Utf8)
      }

  }
}
</script>


<style scoped lang="scss">

.cryptButtons {
  box-sizing: border-box;
  padding: 20px;
}

button {
  margin-left: 8px;
}

.cryptButtonsMain {
  box-sizing: border-box;
  padding: 0px;
  margin-bottom: 20px;
}

.unCryptButtons {
  box-sizing: border-box;
  padding: 20px;
}
</style>
