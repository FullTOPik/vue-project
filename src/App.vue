<template>
  <div id="app">
    <div class="block">
      <div class="first-input-block divider">
        <h1 class="title">Регистрация</h1>
      </div>
      <div class="main">
        <span class="text">Заполните Ваши Данные</span>
        <div>
          <div class="first-input-block">
            <input
              :style="errors.name ? 'border-color: red' : ''"
              :value="data.name"
              @input="(event) => (data.name = event.target.value)"
              class="custom-input"
              placeholder="Имя"
            />
            <input
              :style="errors.email ? 'border-color: red' : ''"
              :value="data.email"
              @input="(event) => (data.email = event.target.value)"
              class="custom-input"
              placeholder="Email"
            />
          </div>
          <div class="second-input-block">
            <input
              :style="errors.rang ? 'border-color: red' : ''"
              :value="data.rang"
              @input="(event) => (data.rang = event.target.value)"
              class="custom-input"
              placeholder="Должность"
            />
          </div>
          <div class="next-input-block">
            <div>
              <input
                :checked="!!polisy"
                :style="errors.password ? 'border-color: red' : ''"
                :type="show.isPasswordShow ? 'password' : 'text'"
                v-model="data.password"
                class="custom-input"
                placeholder="Пароль"
              />
              <img
                class="image"
                :value="data.repeatPassword"
                @click="show.isPasswordShow = !show.isPasswordShow"
                :class="{ hide: show.isPasswordShow }"
                :src="show.isPasswordShow ? images.hideImage : images.showImage"
              />
            </div>
            <div>
              <input
                :style="errors.repeatPassword ? 'border-color: red' : ''"
                v-model="data.repeatPassword"
                :type="show.isRepeatPasswordShow ? 'password' : 'text'"
                class="custom-input"
                placeholder="Повторите пароль"
              />
              <img
                class="image"
                @click="show.isRepeatPasswordShow = !show.isRepeatPasswordShow"
                :class="{ hide: show.isRepeatPasswordShow }"
                :src="
                  show.isRepeatPasswordShow
                    ? images.hideImage
                    : images.showImage
                "
              />
            </div>
          </div>
        </div>
      </div>
      <div style="width: 100%; padding: 15px">
        <div style="display: flex; justify-content: flex-start; margin: 15px">
          <div
            @click="isToggleActive = !isToggleActive"
            class="toggle"
            :class="{ active: isToggleActive }"
          >
            <div class="circle" />
          </div>
          <div
            style="
              display: flex;
              flex-direction: column;
              justify-content: flex-start;
            "
          >
            <span class="text"
              >Хотите чтобы ваш профиль видели другие участники платформы?</span
            >
            <span
              style="
                margin-left: -10px;
                margin-top: 10px;
                color: #696977;
                font-size: 14px;
              "
              >Включает профиль для просмотра другими пользователями по
              ссылке</span
            >
          </div>
        </div>
        <div style="display: flex">
          <div
            style="display: flex; align-items: flex-start; margin-left: 15px"
          >
            <input
              :style="errors.repeatPassword ? 'border-color: red' : ''"
              v-model="data.polisy"
              style="width: 19px; height: 17px"
              type="checkbox"
            />
            <div style="width: 480px; display: flex">
              <span
                style="
                  font-size: 14px;
                  wont-weight: 400;
                  text-align: initial;
                  margin-left: 30px;
                "
                >Регистрируясь, Вы соглашаетесь с
                <a class="link" href="#">политикой конфиденциальности</a> и
                обработкой
                <a class="link" href="#">персональных данных</a></span
              >
            </div>
          </div>
          <button @click="request" class="registration">
            Зарегистрироваться
          </button>
        </div>
      </div>
    </div>

    <div
      v-if="loading || message"
      style="
        display: flex;
        align-items: center;
        justify-content: center;
        width: 100vw;
        height: 100vh;
        z-index: 100;
        position: absolute;
        opacity: 0.2;
        background: black;
        margin: 0;
      "
    ></div>
    <div
      v-if="loading || message"
      style="
        display: flex;
        align-items: center;
        justify-content: center;
        width: 100vw;
        height: 100vh;
        z-index: 100;
        position: absolute;
        margin: 0;
      "
    >
      <div
        v-if="message"
        style="
          width: 300px;
          height: 120px;
          padding: 15px;
          background: white;
          border-radius: 12px;
          display: flex;
          align-items: center;
          justify-content: space-between;
          flex-direction: column;
          padding-top: 30px;
        "
      >
        <h3>{{ message }}</h3>
        <button
          class="registration"
          style="margin: 0; width: 100px; height: 39px; font-size: 20px"
          @click="message = ''"
        >
          Ок
        </button>
      </div>
      <div
        v-if="loading"
        class="loader"
        style="
          width: 100px;
          height: 100px;
          border-top: 2px dashed black;
          border-radius: 50%;
          animation: 2s linear infinite rotate;
        "
      />
    </div>
  </div>
</template>

<script>
import hideImage from "./assets/images/hide.svg";
import showImage from "./assets/images/show.svg";
import { ref } from "vue";

export default {
  name: "App",
  methods: {
    async request() {
      let error = false;
      Object.entries(this.data).map(([key, value]) => {
        if (!value) {
          this.errors[key] = "error";
          error = true;
        } else {
          this.errors[key] = "";
        }
      });
      if (error) return;

      const requestData = {
        repeat_password: this.data.repeatPassword,
        username: this.data.name,
        //не хотел инпут на селект переписывать ибо тестовое
        role: Number(this.data.rang),
        password: this.data.password,
        public: this.isToggleActive,
        email: this.data.email,
      };

      //mock request
      await fetch("", requestData);

      this.loading = true;
      await new Promise((resolve) =>
        setTimeout(() => {
          resolve();
        }, 5000)
      );
      this.loading = false;
      this.message = "You was registration success!";
      this.data = {
        name: "",
        email: "",
        rang: "",
        password: "",
        repeatPassword: "",
        polisy: true,
      };
    },
  },
  data() {
    return {
      images: { hideImage, showImage },
    };
  },
  setup() {
    const isToggleActive = ref(true);
    const data = ref({
      name: "",
      email: "",
      rang: "",
      password: "",
      repeatPassword: "",
      polisy: true,
    });
    const errors = ref({
      name: "",
      email: "",
      rang: "",
      password: "",
      repeatPassword: "",
      polisy: "",
    });
    const show = ref({
      isPasswordShow: true,
      isRepeatPasswordShow: true,
    });
    const loading = ref(false);
    const message = ref("");

    return {
      message,
      data,
      show,
      isToggleActive,
      loading,
      errors,
    };
  },
  // methods: {
  //   change() {
  //     isToggleActive.value = false;
  //   },
  // },
  // components: {
  //   HelloWorld
  // }
};
</script>

<style>
#app {
  display: flex;
  align-items: center;
  justify-content: center;
  background: #f2f2f2;
  height: 100vh;
  width: 100vw;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #000000;
}

.custom-input {
  height: 39px;
  width: 450px;
  border: 1px solid #e6e6eb;
  border-radius: 11px;
  padding-left: 10px;
}

.custom-input::placeholder {
  color: #9292a0;
}

.toggle {
  background: grey;
  width: 39px;
  height: 19px;
  border-radius: 10px;
}

.circle {
  background: white;
  width: 19px;
  height: 19px;
  border-radius: 10px;
}

.block {
  width: 980px;
  height: 517px;
  background: white;
  border-radius: 15px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-start;
}

.first-input-block {
  margin: 15px;
  width: 940px;
  display: flex;
  justify-content: space-between;
}

.second-input-block {
  margin: 15px;
  width: 940px;
  display: flex;
  justify-content: flex-end;
  margin-top: 31px;
}

.next-input-block {
  margin: 15px;
  display: flex;
  width: 940px;
  justify-content: space-between;
  margin-top: 31px;
}

.title {
  color: #000000;
  margin-left: 15px;
}

.text {
  color: #000000;
  font-weight: 500;
  width: 100%;
  text-align: start;
  margin-left: 20px;
}

.toggle.active {
  background: #3586ff;
  display: flex;
  justify-content: flex-end;
}

.divider {
  width: 100%;
  border-bottom: 1px solid #d9d9d9;
}

@keyframes rotate {
  from {
    transform: rotate(0deg);
  }

  20% {
    transform: rotate(70deg);
  }

  40% {
    transform: rotate(90deg);
  }

  60% {
    transform: rotate(140deg);
  }

  80% {
    transform: rotate(300deg);
  }

  to {
    transform: rotate(360deg);
  }
}

.link {
  color: #3586ff;
  text-decoration: none;
}

.main {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  justify-content: flex-start;
  margin-left: 10px;
  margin-right: 10px;
  border-bottom: 1px solid #d9d9d9;
}

.registration {
  margin-left: auto;
  margin-right: 15px;
  color: #497ada;
  border: none;
  border-radius: 8px;
  background: rgba(73, 122, 218, 0.2);
  width: 302px;
  height: 40px;
}

.registration:hover {
  cursor: pointer;
  background: rgba(73, 122, 218, 0.3);
}

.registration:active {
  box-shadow: 0px 0px 2px 4px rgba(73, 122, 218, 0.1);
}

.image.hide {
  margin-top: 14px;
}

.image {
  position: absolute;
  margin-left: -30px;
  margin-top: 15px;
}

.image:hover {
  cursor: pointer;
}
</style>
