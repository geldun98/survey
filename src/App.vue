<template>
  <div id="app">
    <Register
      v-if="isShow.register"
      @handleRegister="handleRegister"
    ></Register>
    <BoxSurvey
      v-if="isShow.boxsurvey"
      :user="user"
      @handleSurvey="handleSurvey"
    ></BoxSurvey>
    <End v-if="isShow.end" :user="user"></End>
  </div>
</template>

<script>
import BoxSurvey from "./components/BoxSurvey.vue";
import End from "./components/End.vue";
import Register from "./components/Register.vue";
export default {
  name: "App",
  components: { Register, BoxSurvey, End },

  data() {
    return {
      isShow: {
        register: true,
        boxsurvey: false,
        end: false,
      },
      user: {},
    };
  },
  methods: {
    handleRegister(dataUser) {
      this.user = { ...dataUser };
      this.isShow.register = false;
      this.isShow.boxsurvey = true;
    },
    handleSurvey(result) {
      this.user = { ...this.user, ...result };
      this.isShow.boxsurvey = false;
      this.isShow.end = true;
      console.log(this.user);
    },
  },
};
</script>

<style lang="scss">
#app {
}
</style>
