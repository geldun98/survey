<template>
  <div class="register">
    <div class="register-box">
      <div class="logo">
        <img class="logo-image" src="@/assets/logo.png" alt="rikkei.edu.vn" />
      </div>
      <h1 class="register-box-title">Khảo sát thông tin</h1>
      <div class="register-form">
        <div class="register-form-info">
          <span class="text-center-bold"
            >Nhập tên, email, số điện thoại để khảo sát</span
          >
        </div>
        <div class="register-form-input">
          <div class="register-form-input-title">Họ và tên <em>*</em></div>
          <div class="register-form-input-content">
            <input
              v-model="fullname"
              type="text"
              placeholder="Họ và tên"
              @focus="handleFocus('fullname')"
            />
            <div v-if="error.fullname" class="error-message">
              Nhập lại trường này
            </div>
          </div>
        </div>
        <div class="register-form-input">
          <div class="register-form-input-title">Email <em>*</em></div>
          <div class="register-form-input-content">
            <input
              v-model="email"
              type="text"
              placeholder="Email"
              @focus="handleFocus('email')"
            />
            <div v-if="error.email" class="error-message">
              Nhập lại trường này
            </div>
          </div>
        </div>
        <div class="register-form-input">
          <div class="register-form-input-title">Số điện thoại <em>*</em></div>
          <div class="register-form-input-content">
            <input
              v-model="phone"
              type="text"
              placeholder="Số điện thoại"
              @focus="handleFocus('phone')"
            />
            <div v-if="error.phone" class="error-message">
              Nhập lại trường này
            </div>
          </div>
        </div>
        <div class="register-form-submit">
          <button @click="handleSubmit">Bắt đầu khảo sát</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "register-comp",
  data() {
    return {
      fullname: "",
      email: "",
      phone: "",
      error: {
        fullname: false,
        email: false,
        phone: false,
      },
    };
  },
  methods: {
    handleSubmit() {
      if (this.fullname.trim().length < 3) {
        this.error.fullname = true;
      }
      if (!this.validateEmail(this.email)) {
        this.error.email = true;
      }
      if (!this.validatePhone(this.phone)) {
        this.error.phone = true;
      }
      if (!this.error.fullname && !this.error.email && !this.error.phone) {
        let dataUser = {
          fullname: this.fullname,
          email: this.email,
          phone: this.phone,
        };

        this.$emit("handleRegister", dataUser);
      }
    },
    handleFocus(name) {
      this.error[name] = false;
    },
    validateEmail(email) {
      let mailformat =
        /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
      if (email.match(mailformat)) {
        return true;
      }
      return false;
    },
    validatePhone(phone) {
      let phoneformat = /^\d+$/;
      if (phone.match(phoneformat) && phone.length > 8) {
        return true;
      }
      return false;
    },
  },
  mounted() {
    if (localStorage.user) {
      const data = JSON.parse(localStorage.user);
      this.fullname = data.fullname;
      this.email = data.email;
      this.phone = data.phone;
    }
  },
};
</script>
<style lang="scss" scoped>
.register {
  width: 100%;
  min-height: 100vh;
  background-image: url("@/assets/background.png");
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
  &-box {
    max-width: 540px;

    margin: 0 auto;
    &-title {
      text-align: center;
      font-size: 30px;
      font-weight: 400;
      color: #333333;
      padding-bottom: 20px;
    }
  }
}

.logo {
  padding: 50px 0;
  text-align: center;
}
.logo-image {
  max-height: 50px;
}
.text-title {
  display: inline-block;
  font-weight: 600;
  width: 200px;
}
.text-content {
  color: #2c3e50;
}
.text-center-bold {
  text-align: center;
  display: block;
  font-weight: 600;
}
.register-form {
  &-info {
    padding: 10px 5px;
    border-bottom: 1px solid #ecf0f1;
  }
  &-input {
    border-bottom: 1px solid #ecf0f1;
    &-title {
      width: 200px;
      em {
        color: red;
      }
    }
    &-content {
      flex: 1;
    }
    input {
      -webkit-user-select: text;
      -moz-user-select: text;
      -ms-user-select: text;
      user-select: text;
      width: 100%;
      font-size: 16px;
      font-family: "Roboto", sans-serif;
      padding: 6px 12px;
      outline: none;
      border: 1px solid #bdc3c7;
      &:focus {
        border-color: #3498db;
      }
    }
    display: flex;
    align-items: center;
    padding: 10px 5px;
  }
  &-submit {
    padding-top: 20px;
    display: flex;
    justify-content: center;
    button {
      padding: 6px 12px;
      outline: none;
      border: 0;
      color: white;
      background: #3498db;
      font-size: 16px;
      font-family: "Roboto", sans-serif;
      border-radius: 5px;
      cursor: pointer;
    }
  }
  .error-message {
    font-size: 14px;
    padding-top: 5px;
    color: #e74c3c;
  }
  //Responsive
  @mixin maxWidth($width) {
    @media screen and (max-width: $width) {
      @content;
    }
  }
  @mixin minWidth($width) {
    @media screen and (min-width: $width) {
      @content;
    }
  }
  @include maxWidth(767px) {
    .register-form-input-title {
      width: 120px;
    }
  }
}
</style>
