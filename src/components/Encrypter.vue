<template>
  <form name="encform" onsubmit="return false;">
    <div class="main">
      <label for="exampleFormControlTextarea1" class="form-label"> Исходные данные</label>
      <textarea name="box1" v-model="box1" class="form-control" id="exampleFormControlTextarea1" placeholder="Введите текст для зашифровки/расшифровки..." rows="3"></textarea>
      <b-form-textarea name="key" v-model="key" class="key" id="textarea-small" size="sm" placeholder="Введите ключ..."></b-form-textarea>
      <b-form-checkbox v-model="checked" name="check-button" switch> Показывать информацию о шифрах</b-form-checkbox>
      <div class="salts"></div>
      <div class="cryptButtons">
        <span id="help1" class="d-inline-block" tabindex="0">
          <button class="btn btn-primary" @click="VernamCrypt()">Шифр Вернама</button>
          <b-tooltip target="help1" placement="left" v-if="checked === true"
            >Шифр Вернама основан на бинарной логике. Он обладает абсолютной криптографической стойкостью - без знания ключа, расшифровать его невозможно (доказано Клодом Шенноном). Также важное
            уточнение, ключ должен быть больше или равен длинне кодируемого сообщения.
          </b-tooltip>
        </span>
        <span id="help2" class="d-inline-block" tabindex="0">
          <button class="btn btn-primary" @click="CezarCrypt()">Шифр Цезаря</button>
          <b-tooltip target="help2" placement="top" v-if="checked === true"
            >Шифр Цезаря — это вид шифра подстановки, в котором каждый символ в открытом тексте заменяется символом, находящимся на некотором постоянном числе позиций левее или правее него в алфавите.
            Данный шифр используется без ключа
          </b-tooltip>
        </span>
        <span id="help3" class="d-inline-block" tabindex="0"
          ><button class="btn btn-primary" @click="encrypt('RC4')">RC4</button>
          <b-tooltip target="help3" placement="right" v-if="checked === true"
            >Алгоритм RC4 разработан Р.Ривестом специально как генератор потока ключевой информации с ключом переменной длины. Генераторы псевдослучайных чисел, построенные с помощью таких алгоритмов,
            как RC4, как правило, значительно быстрее генераторов, основанных на блочных шифрах. Алгоритм RC4 широко применяется в различных системах защиты информации и компьютерных сетях
          </b-tooltip>
        </span>
      </div>

      <div class="cryptButtonsMain">
        <span id="help4" class="d-inline-block" tabindex="0">
          <button class="btn btn-primary" @click="encrypt('AES')">AES</button
          ><b-tooltip target="help4" placement="left" v-if="checked === true">
            Алгоритм шифрования AES представляет блок данных в виде двумерного байтового массива размером 4 на 4. Длинна ключа кратна 128, 192, 256 байтам. Все операции производятся над отдельными
            байтами массива, а также на независимыми столбцами и строками. Данный алгоритм использует 10, 12 или 14 раундов (в зависимости от длинны ключа)
          </b-tooltip>
        </span>
        <span id="help5" class="d-inline-block" tabindex="0">
          <button class="btn btn-primary" @click="encrypt('DES')">DES</button>
          <b-tooltip target="help5" placement="down" v-if="checked === true">
            DES алгоритм симметричного шифрования, разработанный компанией IBM. Размер блока для DES равен 64 битам. В основе алгоритма лежит сеть Фейстеля с 16 раундами и ключом, имеющим длину 56
            бит. Алгоритм использует комбинацию нелинейных и линейных преобразований.
          </b-tooltip>
        </span>
        <span id="help6" class="d-inline-block" tabindex="0">
          <button class="btn btn-primary" @click="encrypt('TripleDES')">Triple DES</button>
          <b-tooltip target="help6" placement="down" v-if="checked === true">
            Triple DES - симметричный блочный шифр, созданный Уитфилдом Диффи, Мартином Хеллманом и Уолтом Тачманном в 1978 году на основе алгоритма DES с целью устранения главного недостатка
            последнего — малой длины ключа (56 бит), который может быть взломан методом полного перебора ключа. Скорость работы 3DES в 3 раза ниже, чем у DES, но криптостойкость намного выше.
          </b-tooltip>
        </span>
        <span id="help7" class="d-inline-block" tabindex="0">
          <button class="btn btn-primary" @click="encrypt('Rabbit')">Rabbit</button>
          <b-tooltip target="help7" placement="rightbottom" v-if="checked === true">
            Rabbit - шифр, использующий 128-битный ключ и 64-битный инициализирующий вектор. Шифр был разработан с целью использования в программном обеспечении, как обладающий высокой скоростью
            шифрования. При этом скорость шифрования могла достигать 3.7 циклов в байт для процессора Pentium 3 и 10.5 циклов в байт для ARM7. Тем не менее, шифр также оказался быстрым и компактным
            при реализации в аппаратном обеспечении.
          </b-tooltip>
        </span>
      </div>

      <label for="exampleFormControlTextarea2" class="form-label"> Результат программы</label>
      <textarea name="box2" class="form-control" v-model="box2" id="exampleFormControlTextarea2" rows="3" placeholder="Данные после шифрации/дешифрации"></textarea>

      <div class="unCryptButtons1">
        <button class="btn btn-primary" @click="CezarunUncrypt()">Расшифровать шифр Цезаря</button>
        <button class="btn btn-primary" @click="decrypt('AES')">Расшифровать AES</button>
        <button class="btn btn-primary" @click="decrypt('DES')">Расшифровать DES</button>
      </div>

      <div class="unCryptButtons2">
        <button class="btn btn-primary" @click="decrypt('TripleDES')">Расшифровать Triple DES</button>
        <button class="btn btn-primary" @click="decrypt('Rabbit')">Расшифровать Rabbit</button>
        <button class="btn btn-primary" @click="decrypt('RC4')">Расшифровать RC4</button>
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
      checked: false,
      qrlink: 'https://vksitproject.vercel.app/#/',
      qrsize: 300,
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
          throw Error('Функция не найдена')
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

    decrypt(name) {
      try {
        const enc = CryptoJS[name]
        if (!enc) {
          throw Error('Шифр не зарегистрирован')
        }
        if (typeof enc.encrypt !== 'function') {
          throw Error('Функция не найдена')
        }

        this.box2 = enc.decrypt(this.box2, this.key).toString(CryptoJS.enc.Utf8)
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
  margin-bottom: 10px;
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
