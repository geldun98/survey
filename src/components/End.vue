<template>
  <div class="end">
    <div class="end-form">
      <div class="end-logo">
        <img src="@/assets/logo.png" alt="rikkei.edu.vn" />
      </div>
      <h3>Bạn đã hoàn thành khảo sát thành công</h3>
    </div>
  </div>
</template>

<script>
export default {
  name: "end-comp",
  props: {
    user: Object,
  },
  mounted() {
    localStorage.user = JSON.stringify(this.user);
  },
};
</script>

<style lang="scss" scoped>
.end {
  width: 100%;
  min-height: 100vh;
  background-image: url("@/assets/background.png");
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
  position: relative;

  &-form {
    width: 100%;
    max-width: 800px;
    padding: 20px;
    background: #ecf0f1;
    border-radius: 15px;
    margin: 0 auto;
    transform: translateY(50px);
    h3 {
      text-align: center;
      margin-bottom: 15px;
    }
  }
  &-logo {
    text-align: center;
    padding: 15px 0;
    img {
      height: 40px;
    }
  }
  .user-info {
    display: flex;
    &-title {
      font-weight: 500;
      width: 150px;
    }
    margin-bottom: 15px;
  }
}
</style>
