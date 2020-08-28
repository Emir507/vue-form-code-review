<template>
  <div class="container">
    <form class="form" @submit.prevent="submitHandle">
      <h1>Регистрация клиента</h1>

      <div class="personal__data">
        <h2 class='title'>Персональная информация</h2>
        <div class="form-group" :class="{ 'form-group--error': $v.name.$error }">
          <label class="form__label">Имя*</label>
          <input
            class="form__input" 
            v-model.trim="$v.name.$model" 
            :class="{ error : ($v.name.$dirty && !$v.name.required) || !$v.name.alpha, valid : $v.name.required && $v.name.alpha}"
          >
          <small v-if="!$v.name.alpha">Введите буквы</small>
          <small v-if="$v.name.$dirty && !$v.name.required">Введите имя</small>
        </div>

        <div class="form-group" :class="{ 'form-group--error' : $v.surname.$error}">
          <label class="form__label">Фамилия*</label>
          <input 
            v-model="$v.surname.$model"
            class="form__input"
            :class="{ error : ($v.surname.$dirty && !$v.surname.required) || !$v.surname.alpha, valid: $v.surname.required && $v.surname.alpha}"
          >
          <small v-if="!$v.surname.alpha">Введите буквы</small>
          <small v-if="$v.surname.$dirty && !$v.surname.required">Введите фамилию</small>
        </div>

        <div class="form-group">
          <label class="form__label">Отчество</label>
          <input type="text">
        </div>

        <div class="form-group" :class="{ 'form-group--error' : $v.birthDate.$error}">
          <label class="form__label" for="">Дата рождения*</label>
          <input 
            type="text"
            placeholder="ДД.ММ.ГГГГ"
            v-model="$v.birthDate.$model"
            :class="{ error : ($v.birthDate.$dirty && !$v.birthDate.validDate), valid: $v.birthDate.validDate}"
          >
          <small v-if="$v.birthDate.$error">Введите в заданном формате</small>
        </div> 

          <div class='form-group'>
          <label class='form__label'>Номер телефона</label>
          
          <div class='form-group__phone-number'>
            <div>+7</div>
            <input 
            type="text" 
            placeholder='(999) 999-99-99' 
            v-model="phoneNumber"
            @input='acceptNumber'
            >
          </div>
        </div>  

          <div class='form-group'>
          <label class='form__label'>Пол</label>
          <input type="text">
        </div> 

        <div class='form-group'>
          <label class='form__label'>Группа клиентов</label>
          <select multiple class="clients-select">
            <option disabled>Можно выбрать несколько пунктов</option>
            <option v-for="(group, index) in clientGroups" :key='index' value=''>{{ group }}</option>
          </select>
        </div>  
        <div class='form-group'>
          <label class='form__label'>Лечащий врач</label>
          <select name='doctors'>
            <option value="" disabled selected>Выберите лечащего врача</option>
            <option v-for="(doctor, index) in doctors" :key='index' value=''>{{ doctor }}</option>
          </select>
        </div>          
      </div>

      <div class="adress">
        <h2 class='title'>Адрес</h2>
        <div class="form-group">
          <label for="">Индекс</label>
            <input type="text">
        </div>
        <div class="form-group">
          <label for="">Страна</label>
          <input type="text">
        </div>
        <div class="form-group">
          <label for="">Область</label>
          <input type="text">
        </div>
        <div class="form-group" :class="{ 'form-group--error' : $v.city.$error}">
          <label for="">Город*</label>
          <input 
            v-model="$v.city.$model"
            type="text" 
            :class="{ error : ($v.city.$dirty && !$v.city.required) || !$v.city.alpha, valid: $v.city.required && $v.city.alpha}"
          >
          <small v-if="!$v.city.alpha">Введите буквы</small>
          <small v-if="$v.city.$dirty && !$v.city.required">Введите город</small>
        </div>

        <div class="form-group">
          <label for="">Улица</label>
          <input type="text">
        </div>
        <div class="form-group">
          <label for="">Дом</label>
          <input type="text">
        </div>
      </div>

      <div class="passport__data">
        <h2 class="title">Паспорт</h2>
        <div class="form-group" :class="{'form-group--error' : $v.text.$error}">
          <label>Тип документа*</label>
          <select 
            v-model="text"
            :class="{ error: $v.text.$error, valid : !$v.text.$invalid}"
          >
            <option value="" disabled selected>Выберите документ</option>
            <option :value='item' v-for="item in docTypes" :key="item.id">{{ item.type }}</option>
          </select>
          <small v-if="$v.text.$error">Выберите тип документа</small>
        </div>

        <div class="form-group">
          <label for="">Серия</label>
          <input type="text">
        </div>

        <div class="form-group">
          <label for="">Номер</label>
          <input type="text">
        </div>

        <div class="form-group">
          <label for="">Кем выдан</label>
          <input type="text">
        </div>

        <div class="form-group" :class="{ 'form-group--error' : $v.date.$error}">
          <label class="form__label" for="">Дата выдачи*</label>
          <input 
            type="text"
            placeholder="ДД.ММ.ГГГГ"
            v-model="$v.date.$model"
            :class="{ error : ($v.date.$dirty && !$v.date.validDate), valid: $v.date.validDate}"
          >
          <small v-if="$v.date.$error">Введите в заданном формате</small>
        </div> 

      </div>

      <div class="form-control">
        <button class="sumbit-btn" type="submit">Отправить</button>
      </div>

      <ModalComponent v-if="modal" @close="closeModal"/>
    </form>
  </div>
</template>

<script>
import ModalComponent from '@/components/Form/ModalComponent.vue'
import {  minLength, required, alpha, maxLength, numeric } from 'vuelidate/lib/validators'
import moment from 'moment'

export default {
  components: { ModalComponent },
  data() {
    return {
      modal: false,
      name: '',
      surname: '',
      birthDate: '',
      phoneNumber: '',
      doctors: ['Иванов', 'Захаров', 'Чернышева'],
      clientGroups: ['VIP', 'Проблемные', 'ОМС'],
      docTypes: [
        { id: 1,type: 'Паспорт' },
        { id: 2,type: 'Свидетельство о рождении' },
        { id: 3,type: 'Вод. удостоверение' } 
      ],
      date: '',
      text: {
        id: null,
        type: null
      },
      city: '',
      sumbitStatus: null,
      birthDate: '',
    }
  },
  validations: {
    name: {
      required,
      alpha: val => /^[а-яёa-z]*$/i.test(val)
    },
    surname: {
      required,
      alpha: val => /^[а-яёa-z]*$/i.test(val)
    },
    text: {
      id: { required },
      type: { required }
    },
    birthDate: {
      validDate : val => moment(val, 'DD.MM.YYYY', true).isValid(),
      required
    },
    date: {
      validDate : val => moment(val, 'DD.MM.YYYY', true).isValid(),
      required
    },
    city: {
      required,
      alpha: val => /^[а-яёa-z]*$/i.test(val)
    },
  },
  methods: {
    closeModal() {
      document.body.classList.remove('modal');
      this.modal = false
    },
    submitHandle() {
      this.$v.$touch();
      if (this.$v.$error){
        return
      } else {
        this.modal = true
        document.body.classList.add('modal');
      }
    },
    acceptNumber() {
      let x = this.phoneNumber.replace(/\D/g, '').match(/(\d{0,3})(\d{0,3})(\d{0,2})(\d{0,2})/); // заменяет все буквы на пустое место. Возвращает массив из 3 элементов в которые собирает по 3 элемента
      this.phoneNumber = !x[2] ? x[1] : '(' + x[1] + ') ' + x[2] + (x[3] ? '-' + x[3] : '') + (x[4] ? '-' + x[4] : ''); 
    },
  }
}
</script>

<style lang="scss">
  @import '../assets/mixin/__mixin.scss';
  .container {
    padding: 30px 0;
  }
  .form {
    h1 {
      padding: 20px 0;
      background-color: rgb(235, 235, 235);
      border-bottom: 1px solid rgb(199, 199, 199);
      @include phones {
        font-size: 28px;
      }
      @include iPhone4 {
        font-size: 25px;
      }
    }
    box-shadow: 0 0 25px 0px black;
    width: 400px;
    margin: 0 auto;
    background-color: #fff;
    @include phones {
      width: 350px;
    }
    @include iPhone4 {
      width: 92%;
    }
  }
  .personal__data {
    margin-bottom: 30px;
    padding-top: 14px;
  }
  .title {
    margin-bottom: 15px;
    @include phones {
      // margin-bottom: 15px;
      font-size: 23px;
    }
    @include iPhone4 {
      // margin-bottom: 15px;
      font-size: 20px;
    }
  }
  .form-group__phone-number {
    display: flex;
    div {
      background-color: rgb(228, 228, 228);
      padding: 10px 15px;
      border-radius: 5px;
      margin-right: 10px;
    }
    input {
      flex: 1;
    }
  }
  .adress {
    border-top: 1px solid #000000;
    margin-bottom: 30px;    
    padding-top: 14px;
  }
  .passport__data {
    border-top: 1px solid #000000;
    margin-bottom: 20px;
    padding-top: 14px;
  }
  .form-group {
    display: flex;
    flex-direction: column;
    padding: 0 20px;
    @include phones {
      padding: 0 10px;
    }
    @include iPhone4{
      padding: 0 10px;
    }
    &:not(:last-child) {
      margin-bottom: 20px;
    }
    label {
      text-align: left;
      font-size: 16px;
      @include phones {
        font-size: 17px;
      }
      @include iPhone4 {
        font-size: 14px;
      }
    }
    input {
      padding: 10px;
      transition: all .4s;
    }
  }
  .error {
    border: 3px solid rgb(245, 138, 138);
    border-radius: 5px;
    background-color: rgb(245, 138, 138);
  }
  .valid {
    border: 3px solid rgb(138, 245, 200);
    border-radius: 5px;
    background-color: rgb(138, 245, 200);
  }
  .form-group--error {
    color: red;
  }
  .form-control {
    padding: 20px;
    // background-color: rgb(199, 199, 199);
    background-color: rgb(235, 235, 235);
    display: flex;
    justify-content: flex-end;
    border-top: 1px solid rgb(199, 199, 199);
  }
  .sumbit-btn {
    background-color: rgb(0, 183, 255);
    padding: 10px;
  }
  .clients-select {
    background-color: rgb(228, 228, 228);
    font-family: Gagalin ,serif;
  }
</style>