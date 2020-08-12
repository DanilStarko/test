<template>
   <div class="create-user__body">
      <div class="create-user__title">
         <h1>
            Заполните форму
         </h1>
      </div>
      
      <form class="create-user__form" @submit.prevent="submitHandler">
         <div class="create-user__subtitle">
            <h3>Личные данные:</h3>
         </div>
         <div class="create-user__input">
            <input 
               type="text" 
               class="input" 
               placeholder="Фамилия"
               v-model.trim="$v.surname.$model"
            >
            <span class="error" v-if="$v.surname.$dirty && !$v.surname.required">Поле обязательно для заполнения</span>
         </div>
         <div class="create-user__input">
            <input 
               type="text" 
               class="input" 
               placeholder="Имя"
               v-model.trim="$v.name.$model"
            >
            <span class="error" v-if="$v.name.$dirty && !$v.name.required">Поле обязательно для заполнения</span>
         </div>
         
         <div class="create-user__input">
            <input 
               type="text" 
               class="input" 
               placeholder="Отчество"
               v-model="middleName"
            >
         </div>
         <div class="create-user__input">
            <input 
               type="date" 
               class="input" 
               placeholder="Дата рождения"
               v-model.trim="$v.date.$model"
            >
            <span class="error" v-if="$v.date.$dirty && !$v.date.required">Поле обязательно для заполнения</span>
         </div>
         <div class="create-user__input">
            <input 
               type="tel" 
               class="input" 
               placeholder="Номер телефона"
               v-model.trim="$v.phone.$model"
               @change="setPhone($event.target.value)"
            >
            <span class="error" v-if="$v.phone.$dirty && !$v.phone.required">Поле обязательно для заполнения</span>
            <span class="error" v-else-if="$v.phone.$dirty && phoneError">Телефон должен содержать 11 символов. Сейчас {{phone.length}}</span>
            <span class="error" v-else-if="$v.phone.$dirty && !(phone[0] === '7')">Номер должен начинаться с 7</span>
         </div>
         <div class="create-user__inner">
            <div class="create-user__label">Пол:</div>
            <div class="create-user__gender">
               <input type="radio" name="gender" id="men" value="Мужской" v-model="gender">
               <label for="men">
                  Мужской
               </label>
            </div>
            <div class="create-user__gender">
               <input type="radio" name="gender" id="woman" value="Женский" v-model="gender">
               <label for="woman">
                  Женский
               </label>
            </div>
         </div>
         <div class="create-user__group">
             <select multiple class="create-user__select input" v-model="groupClients">
               <option selected disabled>Выбирите группу клиента</option>
               <option>VIP</option>
               <option>Проблемные</option>
               <option>ОМС</option>
            </select>
            <span class="error" v-if="$v.groupClients.$dirty && !$v.groupClients.required">Выбирите группу клиента</span>
         </div>
         <div class="create-user__group">
             <select class="create-user__select input" v-model="doctor">
               <option selected disabled value="">Выбирите лечащего врача</option>
               <option>Иванов</option>
               <option>Захаров</option>
               <option>Чернышева</option>
            </select>
         </div>
         <div>
            <input type="checkbox" id="check" v-model="sms">
            <label for="check">Не отправлять СМС</label>
         </div>
        <div class="create-user__subtitle">
            <h3 >Адрес:</h3>
         </div>
         <div class="create-user__input">
            <input type="text" class="input" placeholder="Индекс" v-model="index">
         </div>
         <div class="create-user__input">
            <input type="text" class="input" placeholder="Страна" v-model="country">
         </div>
         <div class="create-user__input">
            <input type="text" class="input" placeholder="Область" v-model="region">
         </div>
         <div class="create-user__input">
            <input 
               type="text" 
               class="input" 
               placeholder="Город"
               v-model.trim="$v.city.$model"
            >
             <span class="error" v-if="$v.city.$dirty && !$v.city.required">Поле обязательно для заполнения</span>
         </div>
         <div class="create-user__input">
            <input type="text" class="input" placeholder="Улица" v-model="street">
         </div>
         <div class="create-user__input">
            <input type="text" class="input" placeholder="Дом" v-model="house">
         </div>
         <div class="create-user__subtitle">
            <h3 >Паспорт:</h3>
         </div>
         <div class="create-user__group">
             <select class="create-user__select input" v-model="$v.typeDoc.$model">
               <option selected disabled value="">Тип документа</option>
               <option>Паспорт</option>
               <option>Свидетельство о рождении</option>
               <option>Вод. удостоверение</option>
            </select>
            <span class="error" v-if="$v.typeDoc.$dirty && !$v.typeDoc.required">Выбирите тип документа</span>
         </div>
         <div class="create-user__input">
            <input type="text" class="input" placeholder="Серия" v-model="series">
         </div>
         <div class="create-user__input">
            <input type="text" class="input" placeholder="Номер" v-model="number">
         </div>
         <div class="create-user__input">
            <input type="text" class="input" placeholder="Кем выдан" v-model="issued">
         </div>
         <div class="create-user__input">
            <input 
               type="date" 
               class="input" 
               placeholder="Дата выдачи*"
               v-model.trim="dateOut"
            >
            <span class="error" v-if="$v.dateOut.$dirty && !$v.dateOut.required">Поле обязательно для заполнения</span>
         </div>
         
         <button type="submit" class="create-user__btn">Submit</button>
      </form>
      <div class="create-user__success success-popup" v-if="popup">
         <div class="success-popup__content">
            <div class="success-popup__text">Пользователь успешно добавлен</div>
         </div>
      </div>
   </div>
</template>

<script>

import {required} from 'vuelidate/lib/validators'

export default {
   name: 'createUser',
   data() {
      return {
         phoneError: false,
         name: '',
         surname: '',
         date: '',
         phone: '',
         gender: '',
         groupClients: [],
         city: '',
         typeDoc: '',
         dateOut: '',
         doctor: '',
         sms: '',
         index: '',
         country: '',
         region: '',
         issued: '',
         street: '',
         number: '',
         series: '',
         house: '',
         middleName: '',
         popup: false
      }
   },
   validations: {
      name: {required},
      surname: {required},
      date: {required},
      phone: {required},
      groupClients: {required},
      city: {required},
      typeDoc: {required},
      dateOut: {required}
   },
   methods: {
      setPhone(value) {
         if (value.length !== 11) {
            this.phoneError = true
         } else {
            this.phoneError = false
         }
      },
      submitHandler() {
         if (this.$v.$invalid) {
            this.$v.$touch()
         } else {
            const formData = {
               name:  this.name,
               surname:  this.surname,
               date:  this.date,
               phone:  this.phone,
               gender: this.gender,
               groupClients:  this.groupClients,
               city:  this.city,
               typeDoc:  this.typeDoc,
               dateOut:  this.dateOut,
               doctor: this.doctor,
               sms: this.sms,
               index: this.index,
               country: this.country,
               region: this.region,
               issued: this.issued,
               street: this.street,
               number: this.number,
               series: this.series,
               house: this.house,
               middleName: this.middleName,
            }

            this.name = "",
            this.surname = "",
            this.date = "",
            this.phone = "",
            this.gender = "",
            this.groupClients = [],
            this.city = "",
            this.typeDoc = "",
            this.dateOut = "",
            this.sms = "",
            this.index = "",
            this.country = "",
            this.region = "",
            this.issued = "",
            this.street = "",
            this.number = "",
            this.series = "",
            this.house = "",
            this.doctor = "",
            this.middleName = ""
            this.$v.$reset()

            console.log(formData);

            this.popup = true

            setTimeout(() => this.popup = false, 1500)
         }

         
         
      }
   }
 
}
</script>

<style lang="scss">

.create-user {
   
   &__body {
      max-width: 550px;
      margin: 0px 0px 100px 100px;
      @media screen and (max-width: 768px){
         margin: 0px auto 50px auto;
      }
      @media screen and (max-width: 600px){
         padding: 0 15px;
         width: 100%;
         max-width: 100%;
      }
   }
   &__title {
      margin: 30px 0px 30px 0px;
      font-size: 21px;
      line-height: 27px;
      @media screen and (max-width: 600px){
         font-size: 17px;
         line-height: 21px;
         margin: 20px 0px 30px 0px;
      }
      @media screen and (max-width: 500px){
         font-size: 15px;
         line-height: 18px;
         margin: 20px 0px 30px 0px;
         h1 {
            line-height: 30px;
         }
      }
   }
   &__form {
      width: 100%;
   }
   &__input {
      margin: 0px 0px 15px 0px;
   }
   &__inner {
      display: flex;
      @media screen and (max-width: 500px){
         display: block;
      }
   }
   &__label {
      margin: 0px 50px 0px 0px;
      font-size: 16px;
      line-height: 18px;
      font-weight: 600;
      @media screen and (max-width: 500px){
         display: block;
         margin: 0px 0px 20px 0px;
      }
   }
   &__gender {
      margin: 0 30px 15px 0;
   }
   &__group {
      h5 {
         font-size: 16px;
         line-height: 18px;
         font-weight: normal;
         margin: 10px 0px 0px 0px;
      }
   }
   &__subtitle {
      margin: 50px 0px 30px 0px;
      h3 {
         font-size: 22px;
         line-height: 26px;
         @media screen and (max-width: 550px){
            font-size: 19px;
            line-height: 21px;
         }
      }
      @media screen and (max-width: 550px){
         margin: 50px 0px 20px 0px;
      }
   }
   &__btn {
      font-size: 18px;
      line-height: 22px;
      font-weight: 600;
      width: 100%;
      max-width: 50%;
      margin: 30px auto 0 auto;
      display: block;
      background: transparent;
      border: 1px solid green;
      transition: all 0.3s;
      height: 50px;
      cursor: pointer;
      border-radius: 30px;
      &:hover {
         background: green;
      }
      @media screen and (max-width: 550px){
         max-width: 80%;
      }
   }
   &__select {
      margin: 0px 0px 15px 0px;
   }
}

.input {
   width: 100%;
   border: 1px solid #ccc;
   border-radius: 3px;
   padding: 15px 0 15px 20px;
}

.error {
   font-size: 12px;
   line-height: 14px;
   color: red;
}




.success-popup {
   position: fixed;
   top: 0;
   left: 0;
   width: 100%;
   height: 100%;
   display: flex;
   justify-content: center;
   align-items: center;
		// .success-popup__content
		&__content {
         width: 200px;
         height: 100px;
         border: 1px solid #ccc;
         border-radius: 5px;
         background-color: rgba(204, 204, 204, 0.5);
         display: flex;
         justify-content: center;
         align-items: center;
		}

		// .success-popup__text
		&__text {
         text-align: center;
         font-size: 18px;
         line-height: 22px;

		}
}

</style>
