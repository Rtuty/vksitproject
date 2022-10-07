<template>
  <form name="encform" onsubmit="return false;">
    <div class="main">
      <label for="exampleFormControlTextarea1" class="form-label"> Исходные данные</label>
      <textarea name="box1" v-model="box1" class="form-control" id="exampleFormControlTextarea1" placeholder="Введите текст для зашифровки/расшифровки..." rows="3"></textarea>
      <b-form-textarea name="key" v-model="key" class="key" id="textarea-small" size="sm" placeholder="Введите ключ..."></b-form-textarea>
      <div class="salts">
        <!-- <button class="btn btn-primary" @click="encrypt('SHA256')">SHA256</button>
        <button class="btn btn-primary" @click="encrypt('MD5')">MD5</button>
        <button class="btn btn-primary" @click="encrypt('RIPEMD160')">RIPEMD-160</button> -->
      </div>
      <div class="cryptButtons">
        <button class="btn btn-primary" @click="VernamCrypt()">Шифр Вернама</button>
        <button class="btn btn-primary" @click="CezarCrypt()">Шифр Цезаря</button>
        <button class="btn btn-primary" @click="encrypt('RC4')">RC4</button>
      </div>

      <div class="cryptButtonsMain">
        <button class="btn btn-primary" @click="encrypt('AES')">AES</button>
        <button class="btn btn-primary" @click="encrypt('DES')">DES</button>
        <button class="btn btn-primary" @click="encrypt('TripleDES')">Triple DES</button>
        <button class="btn btn-primary" @click="encrypt('Rabbit')">Rabbit</button>
      </div>

      <label for="exampleFormControlTextarea2" class="form-label"> Результат программы</label>
      <textarea name="box2" class="form-control" v-model="box2" id="exampleFormControlTextarea2" rows="3" placeholder="Данные после шифрации/дешифрации"></textarea>

      <div class="unCryptButtons1">
        <button class="btn btn-primary" @click="CezarunUncrypt()">Расшифровать шифр Цезаря</button>
        <button class="btn btn-primary" @click="AesUncrypt()">Расшифровать AES</button>
        <button class="btn btn-primary" @click="DesUncrypt()">Расшифровать DES</button>
      </div>

      <div class="unCryptButtons2">
        <button class="btn btn-primary" @click="TripleDESUncrypt()">Расшифровать Triple DES</button>
        <button class="btn btn-primary" @click="RabbitUncrypt()">Расшифровать Rabbit</button>
        <button class="btn btn-primary" @click="RC4Uncrypt()">Расшифровать RC4</button>
      </div>
    </div>
  </form>
</template>

<script>
var CryptoJS = require('crypto-js')

export default {
  name: 'EncrypterFunctions',
  data() {
    return {
      box1: null,
      box2: null,
      key: null,
    }
  },
  props: {
    msg: String,
  },
  methods: {
    encrypt(name) {
      try {
        const enc = CryptoJS[name]
        if (!enc) {
          throw Error('Шифр не зарегистрирован')
        }

        if (typeof enc.encrypt !== 'function') {
          throw Error('Нет такой функции')
        }

        this.box2 = enc.encrypt(this.box1, this.key).toString()
      } catch (e) {
        this.$bvModal.msgBoxOk(e.message, {
          title: 'Ошибка',
          size: 'sm',
          buttonSize: 'sm',
          okVariant: 'danger',
          headerClass: 'p-2 border-bottom-0',
          footerClass: 'p-2 border-top-0',
          headerBgVariant: 'danger',
          centered: true,
        })
      }
    },

    // TODO: Внести в функцию encrypt или удалить, так как соли не совсем подходят в концепцию программы
    sha256() {
      this.box2 = CryptoJS.SHA256(this.box1)
    },

    md5() {
      this.box2 = CryptoJS.MD5(this.box1)
    },

    ripemd() {
      this.box2 = CryptoJS.RIPEMD160(this.box1)
    },

    CezarCrypt() {
      let theText = this.box1
      let output = new String()
      let Temp = new Array()
      let Temp2 = new Array()
      let TextSize = theText.length
      for (let i = 0; i < TextSize; i++) {
        let rnd = Math.round(Math.random() * 122) + 68
        Temp[i] = theText.charCodeAt(i) + rnd
        Temp2[i] = rnd
      }
      for (let i = 0; i < TextSize; i++) {
        output += String.fromCharCode(Temp[i], Temp2[i])
      }
      this.box2 = output
    },

    CezarunUncrypt() {
      let theText = this.box2
      let output = new String()
      let Temp = new Array()
      let Temp2 = new Array()
      let TextSize = theText.length
      for (let i = 0; i < TextSize; i++) {
        Temp[i] = theText.charCodeAt(i)
        Temp2[i] = theText.charCodeAt(i + 1)
      }
      for (let i = 0; i < TextSize; i = i + 2) {
        output += String.fromCharCode(Temp[i] - Temp2[i])
      }
      this.box2 = output
    },

    VernamCrypt() {
      var theText = this.box1
      // генератор случайных чисел в заданном диапазоне
      function getRandomInt(min, max) {
        return Math.floor(Math.random() * (max - min)) + min
      }
      var input, output, key
      var inp, k
      input = theText

      key = this.key //prompt('Введите ключ и запишите его - он потребуется для расшифровки сообщения')

      // если длина ключа меньше длины сообщения — говорим пользователю и генерируем свой ключ
      if (key.length < input.length) {
        alert('Ключ короче сообщения, это небезопасно. Скопируйте новый сгенерированный ключ из консоли браузера.')
        key = ''
        // генерируем новый ключ такой же длины, как и сообщение
        for (var i = 0; i < input.length; i++) {
          key += String.fromCharCode(getRandomInt(0, 66535))
        }
        console.log('Скопируйте новый ключ ↓')
        //console.log(key)
        this.key = key
      }
      output = ''
      for (i = 0; i < input.length; i++) {
        inp = input.charCodeAt(i)
        k = key.charCodeAt(i)
        output += String.fromCharCode(inp ^ k)
      }
      this.box2 = output
    },

    // TODO: Написать общую функцию uncrypt
    DesUncrypt() {
      try {
        var bytes = CryptoJS.DES.decrypt(this.box2, this.key)
        this.box2 = bytes.toString(CryptoJS.enc.Utf8)
      } catch (e) {
        this.$bvModal.msgBoxOk(e.message, {
          title: 'Ошибка',
          size: 'sm',
          buttonSize: 'sm',
          okVariant: 'danger',
          headerClass: 'p-2 border-bottom-0',
          footerClass: 'p-2 border-top-0',
          headerBgVariant: 'danger',
          centered: true,
        })
      }
    },

    AesUncrypt() {
      try {
        var bytes = CryptoJS.AES.decrypt(this.box2, this.key)
        this.box2 = bytes.toString(CryptoJS.enc.Utf8)
      } catch (e) {
        this.$bvModal.msgBoxOk(e.message, {
          title: 'Ошибка',
          size: 'sm',
          buttonSize: 'sm',
          okVariant: 'danger',
          headerClass: 'p-2 border-bottom-0',
          footerClass: 'p-2 border-top-0',
          headerBgVariant: 'danger',
          centered: true,
        })
      }
    },

    TripleDESUncrypt() {
      try {
        var bytes = CryptoJS.TripleDES.decrypt(this.box2, this.key)
        this.box2 = bytes.toString(CryptoJS.enc.Utf8)
      } catch (e) {
        this.$bvModal.msgBoxOk(e.message, {
          title: 'Ошибка',
          size: 'sm',
          buttonSize: 'sm',
          okVariant: 'danger',
          headerClass: 'p-2 border-bottom-0',
          footerClass: 'p-2 border-top-0',
          headerBgVariant: 'danger',
          centered: true,
        })
      }
    },

    RabbitUncrypt() {
      try {
        var bytes = CryptoJS.Rabbit.decrypt(this.box2, this.key)
        this.box2 = bytes.toString(CryptoJS.enc.Utf8)
      } catch (e) {
        this.$bvModal.msgBoxOk(e.message, {
          title: 'Ошибка',
          size: 'sm',
          buttonSize: 'sm',
          okVariant: 'danger',
          headerClass: 'p-2 border-bottom-0',
          footerClass: 'p-2 border-top-0',
          headerBgVariant: 'danger',
          centered: true,
        })
      }
    },

    RC4Uncrypt() {
      try {
        var bytes = CryptoJS.RC4.decrypt(this.box2, this.key)
        this.box2 = bytes.toString(CryptoJS.enc.Utf8)
      } catch (e) {
        this.$bvModal.msgBoxOk(e.message, {
          title: 'Ошибка',
          size: 'sm',
          buttonSize: 'sm',
          okVariant: 'danger',
          headerClass: 'p-2 border-bottom-0',
          footerClass: 'p-2 border-top-0',
          headerBgVariant: 'danger',
          centered: true,
        })
      }
    },
  },
}
</script>
<style scoped lang="scss">
.cryptButtons {
  box-sizing: border-box;
  padding: 10px;
}

.key {
  margin-top: 20px;
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
