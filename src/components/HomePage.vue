<template>
  <v-layout>
    <v-flex xs12 sm4 offset-sm4>
      <v-card class="mt-5">
        <v-container fluid>
          <v-layout>
            <v-flex xs12 v-show="step === 1">
              <h1 class="text-xs-center">Регистрация</h1>
              <v-text-field
                label="Фамилия"
                outlined
              ></v-text-field>
              <v-text-field
                label="Имя"
                outlined
              ></v-text-field>
              <v-text-field
                label="Мобильный телефон"
                mask="#######"
                outlined
              ></v-text-field>
              <v-text-field
                label="Или электронная почта"
                outlined
              ></v-text-field>
              <p class="text-xs-center">Нажимая на кнопку "Зарегистрироваться", вы соглашаетесь с <a>Условиями использования</a> и <a>Политикой конфиденциальности</a></p>
              <div class="text-xs-center">
                <v-btn color="primary" @click="step++">Зарегистрироваться</v-btn>
              </div>
            </v-flex>
            <v-flex xs12 v-show="step === 2">
              <h1 class="text-xs-center">Регистрация</h1>
              <p>Подтверите свой номер телефона. Введите отправленный код.</p>
              <v-text-field
                label="Код подтверждения"
              ></v-text-field>
              <div class="text-xs-center">
                <v-btn color="primary" @click="step++">Продолжить</v-btn>
              </div>
            </v-flex>
            <v-flex xs12 v-show="step === 3">
              <h1 class="text-xs-center">Регистрация</h1>
              <v-text-field
                label="Фамилия"
              ></v-text-field>
              <v-text-field
                label="Имя"
              ></v-text-field>
              <v-text-field
                label="Отчество"
              ></v-text-field>

              <v-radio-group v-model="radios">
                <template slot="label">
                  <div>Пол</div>
                </template>
                <v-radio value="Google">
                  <template slot="label">
                    <div>Мужской</div>
                  </template>
                </v-radio>
                <v-radio value="Duckduckgo">
                  <template slot="label">
                    <div>Женский</div>
                  </template>
                </v-radio>
              </v-radio-group>

              <v-menu
                v-model="dateMenu"
                :close-on-content-click="false"
                :nudge-right="40"
                lazy
                transition="scale-transition"
                offset-y
                full-width
                min-width="290px"
              >
                <v-text-field
                  slot="activator"
                  v-model="dateFormatted"
                  label="Дата рождения"
                  readonly
                  @blur="date = parseDate(dateFormatted)"
                ></v-text-field>
                <v-date-picker v-model="date" @input="setDate" locale="ru-ru" first-day-of-week="1" scrollable></v-date-picker>
              </v-menu>

              <v-textarea
                label="Место рождения"
              ></v-textarea>
              <v-text-field
                label="Гражданство"
              ></v-text-field>
              <v-combobox
                label="Документ"
                :items="docTypes"
              ></v-combobox>
              <v-text-field
                label="Серия"
              ></v-text-field>
              <v-text-field
                label="Номер"
              ></v-text-field>
              <v-textarea
                label="Кем выдан"
              ></v-textarea>

              <v-menu
                v-model="dateMenu2"
                :close-on-content-click="false"
                :nudge-right="40"
                lazy
                transition="scale-transition"
                offset-y
                full-width
                min-width="290px"
              >
                <v-text-field
                  slot="activator"
                  v-model="dateFormatted2"
                  label="Дата выдачи"
                  readonly
                  @blur="date2 = parseDate(dateFormatted2)"
                ></v-text-field>
                <v-date-picker v-model="date2" @input="setDate" locale="ru-ru" first-day-of-week="1" scrollable></v-date-picker>
              </v-menu>



              <v-text-field
                label="Номер"
              ></v-text-field>

              <div class="text-xs-center">
                <v-btn color="primary" @click="step++">Продолжить</v-btn>
              </div>
            </v-flex>
            <v-flex xs12 v-show="step === 4">
              <p>Укажите оператора, через которого можно провести двуфакторную авторизацию</p>
              <v-combobox
                :items="authItems"
                label="Способ подтверждения"
              ></v-combobox>
              <div class="text-xs-center">
                <v-btn color="primary" @click="register">Завершить регистрацию</v-btn>
              </div>
            </v-flex>
          </v-layout>
        </v-container>
      </v-card>
    </v-flex>
  </v-layout>
</template>

<script>
export default {
  name: 'HomePage',
  data: vm => ({
    date: null,
    date2: null,
    dateMenu: false,
    dateMenu2: false,
    dateFormatted: vm.formatDate(new Date().toISOString().substr(0, 10)),
    dateFormatted2: vm.formatDate(new Date().toISOString().substr(0, 10)),
    dialog: false,
    dialog2: false,
    dialog3: false,
    dialog4: false,
    dialog5: false,
    img1: require('@/assets/234-man-raising-hand-1-128.png'),
    img2: require('@/assets/154-man-office-worker-2-128.png'),
    img3: require('@/assets/145-man-mechanic-1-128.png'),
    soc: require('@/assets/image.png'),
    step: 1,
    dialog2type: 1,
    docTypes: ['Паспорт гражданина Российской Федерации', 'Заграничный паспорт гражданина Российской Федерации'],
    authItems: ['Сбербанк', 'МТС', 'Ростелеком']
  }),
  watch: {
    date (val) {
      this.dateFormatted = this.formatDate(this.date)
    },
    date2 (val) {
      this.dateFormatted2 = this.formatDate(this.date2)
    }
  },
  methods: {
    openDialog (temp) {
      this.dialog2type = temp
      this.dialog2 = true
    },
    setDate () {
      this.dateMenu = false
      this.dateMenu2 = false
      localStorage.setItem('timetableDate', this.date)
      this.initialize(this.date)
    },
    formatDate (date) {
      if (!date) return null

      const [year, month, day] = date.split('-')
      return `${day}.${month}.${year}`
    },
    parseDate (date) {
      if (!date) return null

      const [day, month, year] = date.split('.')
      return `${year}-${month.padStart(2, '0')}-${day.padStart(2, '0')}`
    },
    register () {
      alert('Регистрация пройдена успешно!')
      this.step = 1
    }
  }
}
</script>

<style scoped>
</style>
