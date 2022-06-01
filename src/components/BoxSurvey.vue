<template>
  <div class="box-survey-container">
    <div class="box-survey">
      <div class="logo">
        <img class="logo-image" src="@/assets/logo.png" alt="rikkei.edu.vn" />
      </div>
      <div class="mode mode1" v-if="count < 2">
        <div class="title-survey">Câu {{ title.id }}: {{ title.content }}</div>
        <div class="main-survey">
          <div
            class="item-input"
            v-for="(item, key, index) in mode1['content']"
            :key="index"
          >
            <input
              v-model="result[count]"
              type="radio"
              :id="mode1['id'] + key"
              :value="item"
            />
            <label :for="mode1['id'] + key">{{ item }}</label>
          </div>
        </div>
      </div>
      <div class="mode mode2" v-if="count >= 2 && count <= listTitle.length">
        <div class="title-survey">Câu {{ title.id }}: {{ title.content }}</div>
        <div class="main-survey">
          <textarea
            class="item-textarea"
            v-model="result[count]"
            placeholder="Nhập nội dung ở đây"
            ref="reftextarea"
            @input="autoResize"
          ></textarea>
        </div>
      </div>
      <div class="loading" v-if="isLoading">
        <div class="loading-title">Đang gửi dữ liệu, vui lòng chờ</div>
        <div class="loading-pin lds-spinner">
          <div></div>
          <div></div>
          <div></div>
          <div></div>
          <div></div>
          <div></div>
          <div></div>
          <div></div>
          <div></div>
          <div></div>
          <div></div>
          <div></div>
        </div>
      </div>
      <div class="next-button" v-if="!isLoading">
        <button @click="handleNext">Tiếp tục</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "box-survey",
  props: {
    user: Object,
  },
  data() {
    return {
      count: 1,
      isLoading: false,

      result: {},
      listTitle: [
        { id: 1, content: "Giảng viên hiện tại của bạn là ai?" },
        {
          id: 2,
          content: "Cảm nhận của bạn về khóa học cho tới thời điểm hiện tại?",
        },
        { id: 3, content: "Điều gì khiến bạn chưa hài lòng?" },
        { id: 4, content: "Những khó khăn trong quá trình học của bạn là gì?" },
        {
          id: 5,
          content:
            "Bạn đã giới thiệu bạn bè, người thân tham gia chương trình tại Rikkei Academy chưa? Nếu có, hãy viết tên bạn ấy ra để cả hai cùng nhận một phần quà nho nhỏ từ Rikkei Academy nhé.",
        },
      ],
      listMode1: [
        {
          id: 1,
          content: {
            A: "Giang Vien A",
            B: "Giang Vien B",
            C: "Giang Vien C",
            D: "Giang Vien D",
          },
        },
      ],
    };
  },
  methods: {
    async handleNext() {
      this.count = this.count + 1;
      if (this.$refs.reftextarea) {
        this.$refs.reftextarea.style.height = "auto";
      }
      if (this.count > this.listTitle.length) {
        this.isLoading = true;
        await this.sendGoogleSheet();

        this.$emit("handleSurvey", this.result);
      }
    },
    autoResize(e) {
      if (e.target.scrollHeight > e.target.clientHeight) {
        this.$refs.reftextarea.style.height = `${e.target.scrollHeight + 5}px`;
      }
    },
    async sendGoogleSheet() {
      const scriptURL =
        "https://script.google.com/macros/s/AKfycbww9ZWefsZUu6-B5xAweKgQdeslYjGaFxnsa6b3r1E4qqiNKKBOk3ZXCn5IhDv-iA3RYA/exec";

      let formData = new FormData();
      formData.append("Fullname", this.user.fullname);
      formData.append("Email", this.user.email);
      formData.append("Phone", this.user.phone);

      formData.append("Q1", this.result[1] || "");
      formData.append("Q2", this.result[2] || "");
      formData.append("Q3", this.result[3] || "");
      formData.append("Q4", this.result[4] || "");
      formData.append("Q5", this.result[5] || "");

      try {
        await fetch(scriptURL, { method: "POST", body: formData });
      } catch (error) {
        console.log(error);
      }
    },
  },

  computed: {
    title() {
      return this.listTitle.find((item) => item.id === this.count);
    },
    mode1() {
      return this.listMode1.find((item) => item.id === this.count);
    },
  },
  watch: {},
};
</script>

<style lang="scss" scoped>
.box-survey-container {
  width: 100%;
  min-height: 100vh;
  background-image: url("@/assets/background.png");
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
}
.box-survey {
  width: 100%;
  max-width: 800px;
  padding: 20px;
  background: #ecf0f1;
  border-radius: 15px;
  margin: 0 auto;
  transform: translateY(50px);
}
.next-button {
  display: flex;
  justify-content: center;
  align-items: center;
  button {
    color: white;
    border: 0;
    outline: 0;
    background-color: #e74c3c;
    font-size: 20px;
    padding: 10px 15px;
    border-radius: 5px;
    cursor: pointer;
    font-family: "Roboto", sans-serif;
  }
}
.title-survey {
  font-size: 20px;
  font-weight: 600;
  margin-bottom: 15px;
}
.item-input {
  margin-bottom: 15px;

  display: flex;
  align-items: center;
  input {
    margin-right: 5px;
  }
}
.item-textarea {
  width: 100%;
  height: auto;
  margin-bottom: 15px;
  padding: 15px;
  font-family: "Roboto", sans-serif;
  font-size: 16px;
  line-height: 1.1;
  resize: none;
  border: 2px solid black;
  outline: none;
  &:focus {
    border-color: #3498db;
  }
}
.logo {
  padding: 20px 0;
  text-align: center;
}
.logo-image {
  max-height: 50px;
}
.lds-spinner {
  color: official;
  display: inline-block;
  position: relative;
  width: 80px;
  height: 80px;
}
.lds-spinner div {
  transform-origin: 40px 40px;
  animation: lds-spinner 1.2s linear infinite;
}
.lds-spinner div:after {
  content: " ";
  display: block;
  position: absolute;
  top: 3px;
  left: 37px;
  width: 6px;
  height: 18px;
  border-radius: 20%;
  background: #e74c3c;
}
.lds-spinner div:nth-child(1) {
  transform: rotate(0deg);
  animation-delay: -1.1s;
}
.lds-spinner div:nth-child(2) {
  transform: rotate(30deg);
  animation-delay: -1s;
}
.lds-spinner div:nth-child(3) {
  transform: rotate(60deg);
  animation-delay: -0.9s;
}
.lds-spinner div:nth-child(4) {
  transform: rotate(90deg);
  animation-delay: -0.8s;
}
.lds-spinner div:nth-child(5) {
  transform: rotate(120deg);
  animation-delay: -0.7s;
}
.lds-spinner div:nth-child(6) {
  transform: rotate(150deg);
  animation-delay: -0.6s;
}
.lds-spinner div:nth-child(7) {
  transform: rotate(180deg);
  animation-delay: -0.5s;
}
.lds-spinner div:nth-child(8) {
  transform: rotate(210deg);
  animation-delay: -0.4s;
}
.lds-spinner div:nth-child(9) {
  transform: rotate(240deg);
  animation-delay: -0.3s;
}
.lds-spinner div:nth-child(10) {
  transform: rotate(270deg);
  animation-delay: -0.2s;
}
.lds-spinner div:nth-child(11) {
  transform: rotate(300deg);
  animation-delay: -0.1s;
}
.lds-spinner div:nth-child(12) {
  transform: rotate(330deg);
  animation-delay: 0s;
}
@keyframes lds-spinner {
  0% {
    opacity: 1;
  }
  100% {
    opacity: 0;
  }
}
.loading {
  &-title {
    font-size: 20px;
    font-weight: 600;
    margin-bottom: 20px;
  }
  display: flex;
  justify-content: center;
  flex-direction: column;
  align-items: center;
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
}
</style>
