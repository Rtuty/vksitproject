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

      <p>
        <button class="btn btn-primary"
                @click="sha256()">
          SHA256
        </button>

        <button class="btn btn-primary"
                @click="VernamCrypt()">
          Шифр Вернама
        </button>

        <button class="btn btn-primary"
                @click="CezarEncrypt()">
          Шифр Цезаря
        </button>

        <button class="btn btn-primary"
                @click="AesCrypt()">
          AES
        </button>
        <!--        <button class="btn btn-primary" onClick="this.form.box2.value=AesCrypt(this.form.box1.value);">AES</button>-->
      </p>

      <label for="exampleFormControlTextarea2"
             class="form-label">
        Результат программы</label>
      <textarea name="box2"
                class="form-control"
                v-model="box2"
                id="exampleFormControlTextarea2"
                rows="3"
                placeholder="Данные после шифрации/дешифрации"></textarea>
      <p>
        <button class="btn btn-primary"
                @click="CezarunEncrypt()">
          Расшифровать шифр Цезаря
        </button>
        <button class="btn btn-primary"
                @click="AesEncrypt()">
          Расшифровать AES
        </button>
      </p>
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
       CezarEncrypt() {
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
       CezarunEncrypt() {
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
        this.box2 = CryptoJS.AES.encrypt(this.box1, '1234').toString()
      },

      AesEncrypt() {
        var bytes  = CryptoJS.AES.decrypt(this.box2, '1234');
        this.box2 = bytes.toString(CryptoJS.enc.Utf8);
      },

      //worked but useless :P TODO: do something with this donkey shit
      sha256() {
         let theText = this.box1
         this.box2 = sha256(theText)
      }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
</style>
