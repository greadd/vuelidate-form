<template>
  <form class="client__form" @submit.prevent="submit" novalidate>
    <h1 class="client__form-title">Личная информация</h1>
    <label for="last-name">Фамилия*</label>
    <small class="error-text" v-if="$v.lastName.$error">Введите фамилию</small>
    <input
      type="text"
      id="last-name"
      v-model="lastName"
      :class="{ invalid: $v.lastName.$dirty && !$v.lastName.required || $v.lastName.$dirty && !$v.lastName.isText}"
    />
    <label for="first-name">Имя*</label>
    <small class="error-text" v-if="$v.firstName.$error">Введите имя</small>
    <input
      type="text"
      id="first-name"
      v-model="firstName"
      :class="{ invalid: $v.firstName.$dirty && !$v.firstName.required || $v.firstName.$dirty && !$v.firstName.isText }"
    />
    <label for="add-name">Отчество</label>
    <small class="error-text" v-if="$v.addName.$error">Введите отчество</small>
    <input
      type="text"
      id="add-name"
      v-model="addName"
      :class="{ invalid: this.addName.length && !$v.addName.isText} "
    />

    <label for="birthday">Дата рождения*</label>
    <small class="error-text" v-if="$v.birthday.$error">Укажите верную дату рождения</small>
    <input
      type="date"
      id="birthday"
      v-model="birthday"
      :class="{
        invalid:
          ($v.birthday.$dirty && !$v.birthday.required) ||
          !$v.birthday.minValue,
      }"
    />
    <div class="gender">
      <span>Пол:</span>
      <label for="male">М</label>
      <input type="radio" id="male" value="М" v-model="gender" />
      <label for="female">Ж</label>
      <input type="radio" id="female" value="Ж" v-model="gender" />
    </div>

    <label for="phone">Номер телефона*</label>
    <small>Формат: 79001002003</small>
    <small class="error-text" v-if="$v.phone.$error">Введите корректный номер телефона</small>
    <input
      type="text"
      id="phone"
      placeholder="7XXXXXXXXXX"
      v-model="phone"
      :class="{
        invalid:
          ($v.phone.$dirty && !$v.phone.required) ||
          ($v.phone.$dirty && !$v.phone.isPhone),
      }"
    />

    <label for="client-group">Группа клиентов*</label>
    <!-- TODO: сделать валидацию и мультиселектор -->
    <small>Выбранные группы: {{clientGroups.join(', ')}}</small>
    <small class="error-text" v-if="$v.clientGroups.$error">Выберите хотя бы одну группу</small>
    <select
      multiple
      id="client-group"
      v-model="clientGroups"
      :class="{ invalid: $v.clientGroups.$dirty && !$v.clientGroups.required }"
    >
      <option v-for="clientGroup in clientGroupsList" :key="clientGroup">
        {{
        clientGroup
        }}
      </option>
    </select>
    <label for="doctor">Лечащий врач</label>
    <select id="doctor" v-model="doctor">
      <option v-for="doctor in doctors" :key="doctor">{{ doctor }}</option>
    </select>
    <label for="sms">
      Не отправлять смс
      <input type="checkbox" id="sms" v-model="noSms" />
    </label>

    <h1>Адрес</h1>
    <label for="index">Индекс</label>
    <small class="error-text" v-if="$v.index.$error">Введите корректный индекс (6 цифр)</small>
    <input
      type="text"
      id="index"
      v-model="index"
      :class="{
        invalid: (!$v.index.numeric || this.index && !$v.index.minLength || this.index && !$v.index.maxLength),
      }"
    />
    <label for="country">Страна</label>
    <small class="error-text" v-if="$v.country.$error">Название страны содержит только буквы</small>
    <input
      type="text"
      id="country"
      v-model="country"
      :class="{
        invalid: this.country.length && !$v.country.isText,
      }"
    />
    <label for="region">Область</label>
    <small class="error-text" v-if="$v.country.$error">Название области содержит только буквы</small>
    <input
      type="text"
      id="region"
      v-model="region"
      :class="{
        invalid: this.region.length && !$v.region.isText,
      }"
    />
    <label for="city">Город*</label>
    <small class="error-text" v-if="$v.city.$error">Укажите город</small>
    <input
      type="text"
      id="city"
      v-model="city"
      :class="{ invalid: $v.city.$dirty && !$v.city.required || $v.city.$dirty && !$v.city.isText }"
    />
    <label for="street">Улица</label>
    <input type="text" id="street" v-model="street" />
    <label for="apps">Дом</label>
    <small class="error-text" v-if="$v.apps.$error">Введите номер дома</small>
    <input
      type="text"
      id="apps"
      v-model="apps"
      :class="{ invalid: $v.apps.$dirty && !$v.apps.numeric }"
    />

    <h1>Документ</h1>
    <label for="type">Тип документа*</label>
    <small class="error-text" v-if="$v.doctType.$error">Выберите тип документа</small>
    <select
      id="type"
      v-model="doctType"
      :class="{ invalid: $v.doctType.$dirty && !$v.doctType.required }"
    >
      <option v-for="doctType in types" :key="doctType">{{ doctType }}</option>
    </select>
    <label for="serie">Серия</label>
    <input type="text" id="serie" v-model="serie" />
    <label for="number">Номер</label>
    <small class="error-text" v-if="$v.number.$error">Номер документа должен содержать 6 цифр</small>
    <input
      type="text"
      id="number"
      v-model="number"
      :class="{
        invalid: (!$v.number.numeric || this.number && !$v.number.minLength || this.number && !$v.number.maxLength),
      }"
    />
    <label for="whom">Кем выдан</label>
    <textarea type="text" id="whom" v-model="whom" />
    <label for="date">Дата выдачи*</label>
    <small class="error-text" v-if="$v.date.$error">Укажите верную дату выдачи документа</small>
    <input
      type="date"
      id="date"
      v-model="date"
      :class="{
        invalid: ($v.date.$dirty && !$v.date.required) || !$v.date.minValue,
      }"
    />
    <button class="submit-btn" type="submit">Создать</button>
    <div class="message" v-if="message">
      <h3>{{message}}</h3>
    </div>
  </form>
</template>

<script>
import {
  required,
  numeric,
  minLength,
  maxLength,
} from "vuelidate/lib/validators";
export default {
  data: () => ({
    doctors: ["Иванов", "Захаров", "Чернышова"],
    clientGroupsList: ["VIP", "Проблемные", "ОМС"],
    clientGroups: [],
    lastName: "",
    firstName: "",
    addName: "",
    birthday: "",
    gender: null,
    phone: "",
    clientGroup: "",
    doctor: "",
    noSms: false,
    index: "",
    country: "",
    region: "",
    city: "",
    street: "",
    apps: "",
    types: ["Паспорт", "Свидетельство о рождении", "Вод. удостоверение"],
    doctType: "",
    serie: "",
    number: "",
    whom: "",
    date: "",
    message: "",
  }),
  validations: {
    lastName: {
      required,
      isText: (value) => /^[a-zа-яё-]+$/i.test(value),
    },
    firstName: {
      required,
      isText: (value) => /^[a-zа-яё-]+$/i.test(value),
    },
    addName: {
      isText: (value) => (value.length ? /^[a-zа-яё-]+$/i.test(value) : true),
    },
    birthday: {
      required,
      minValue: (value) => value < new Date().toISOString(),
    },
    phone: {
      required,
      isPhone: (value) => /^7[0-9]{10}$/.test(value),
    },
    clientGroups: {
      required,
    },
    index: {
      numeric,
      minLength: minLength(6),
      maxLength: maxLength(6),
    },
    country: {
      isText: (value) => (value.length ? /^[a-zа-яё-]+$/i.test(value) : true),
    },
    city: {
      required,
      isText: (value) => (value.length ? /^[a-zа-яё-]+$/i.test(value) : true),
    },
    region: {
      isText: (value) => (value.length ? /^[a-zа-яё-]+$/i.test(value) : true),
    },
    apps: {
      numeric,
    },
    doctType: {
      required,
    },
    serie: {},
    number: {
      numeric,
      minLength: minLength(6),
      maxLength: maxLength(6),
    },
    date: {
      required,
      minValue: (value) => value < new Date().toISOString(),
    },
  },
  methods: {
    submit() {
      this.$v.$touch();
      if (this.$v.$invalid) {
        this.message = "Ошибка! Проверьте введённые данные.";
        setTimeout(() => {
          this.message = "";
        }, 3000);
      } else {
        this.message = `Клиент ${this.lastName} ${this.firstName} успешно создан!`;
        setTimeout(() => {
          this.message = "";
        }, 3000);
        this.lastName = this.firstName = this.addName = this.birthday = this.gender = this.phone = this.doctor = this.index = this.country = this.region = this.city = this.street = this.apps = this.doctType = this.serie = this.number = this.whom = this.date =
          "";
        this.noSms = false;
        this.clientGroups = [];
        this.$v.$reset();
        this.$emit("created", {
          lastName: this.lastName,
          firstName: this.firstName,
        });
      }
    },
  },
};
</script>

<style scoped lang="scss">
@import "@/assets/vars";
input,
select,
textarea {
  @extend %info-input;
}
.invalid {
  border: 1px solid rgb(255, 152, 152);
  background-color: rgb(255, 189, 189);
  transition: all 0.3s;
  &:hover,
  &:focus {
    border: 1px solid rgb(255, 92, 92);
  }
}
.error-text {
  color: red;
}
.client__form {
  display: flex;
  flex-direction: column;
  padding: 30px;
  border-radius: 10px;
  box-shadow: 0 2px 18px #ccc;
}

.submit-btn {
  margin: 20px 20%;
  border-radius: 10px;
  outline: none;
  padding: 10px;
  cursor: pointer;
  background-color: rgb(36, 56, 231);
  color: white;
  font-size: 20px;
  font-weight: 600;
  letter-spacing: 1px;
  transition: all 0.2s ease-in-out;
  border: none;
  &:hover {
    background-color: rgb(66, 84, 250);
    color: white;
  }
}
.gender input,
.gender label {
  margin-left: 10px;
}
textarea {
  resize: none;
}
.message {
  text-align: center;
  background-color: #ccc;
  border-radius: 10px;
}
@media (max-width: 767px) {
  h1 {
    font-size: 24px;
  }
}
</style>
