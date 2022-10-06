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

      <div class="salts">
        <button class="btn btn-primary"
                @click="sha256()">
          SHA256
        </button>
        <button class="btn btn-primary"
                @click="md5()">
          MD5
        </button>
        <button class="btn btn-primary"
                @click="ripemd()">
          RIPEMD-160
        </button>
      </div>
      <div class="cryptButtons">
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
        <button class="btn btn-primary"
                @click="RabbitCrypt()">
          Rabbit
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

      <div class="unCryptButtons1">
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
      </div>

      <div class="unCryptButtons2">
        <button class="btn btn-primary"
                @click="TripleDESUncrypt()">
          Расшифровать Triple DES
        </button>
        <button class="btn btn-primary"
                @click="RabbitUncrypt()">
          Расшифровать Rabbit
        </button>
      </div>


    </div>
  </form>
</template>

<script>
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

    sha256() {
      this.box2 = CryptoJS.SHA256(this.box1);
    },

    md5() {
      this.box2 = CryptoJS.MD5(this.box1);
    },

    ripemd() {
      this.box2 = CryptoJS.RIPEMD160(this.box1);
    },



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



      AesCrypt() {
        this.box2 = CryptoJS.AES.encrypt(this.box1, 'secret key').toString()
      },

      DesCrypt() {
         this.box2 = CryptoJS.DES.encrypt(this.box1, 'secret key').toString();
      },

      TripleDESCrypt() {
        this.box2 = CryptoJS.TripleDES.encrypt(this.box1, "Secret Passphrase").toString();
      },

      RabbitCrypt() {
        this.box2 = CryptoJS.Rabbit.encrypt(this.box1, "secret key").toString();
      },



      DesUncrypt() {
        var bytes = CryptoJS.DES.decrypt(this.box2, 'secret key');
        this.box2 = bytes.toString(CryptoJS.enc.Utf8);
      },

      AesUncrypt() {
        var bytes  = CryptoJS.AES.decrypt(this.box2, 'secret key');
        this.box2 = bytes.toString(CryptoJS.enc.Utf8);
      },

      TripleDESUncrypt() {
        var bytes = CryptoJS.TripleDES.decrypt(this.box2, "Secret Passphrase");
        this.box2 = bytes.toString(CryptoJS.enc.Utf8)
      },

      RabbitUncrypt() {
        var bytes = CryptoJS.Rabbit.decrypt(this.box2, "secret key")
        this.box2 = bytes.toString(CryptoJS.enc.Utf8)
      }

  }
}
</script>
<style scoped lang="scss">

.cryptButtons {
  box-sizing: border-box;
  padding: 10px;
}

.salts {
  box-sizing: border-box;
  padding-top: 20px;
}

.cryptButtonsMain {
  box-sizing: border-box;
  padding: 0px;
  margin-bottom: 20px;
}

button {
  margin-left: 8px;
}

.unCryptButtons1 {
  padding-top: 20px;
}

.unCryptButtons2 {
  padding-top: 10px;
  padding-bottom: 10px;
}

</style>
