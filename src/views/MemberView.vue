<template>
    <div>
      <h1>會員登入</h1>
      <form @submit.prevent="login">
        <label for="memberPhoneNumber">手機號碼:</label>
        <input type="text" id="memberPhoneNumber" v-model="memberPhoneNumber" required>
  
        <label for="memberPassword">密碼:</label>
        <input type="password" id="memberPassword" v-model="memberPassword" required>
  
        <label for="verification">驗證碼:</label>
        <input type="text" id="verification" v-model="verification" required>
        <img :src="captchaImage" alt="驗證碼圖像">
  
        <button type="submit">登入</button>
      </form>
  
      <div class="test">
        <label for="memberPhoneNumber">使用者輸入的手機號碼:</label>
        {{ memberPhoneNumber }}
        <label for="memberPassword">使用者輸入的密碼:</label>
        {{ memberPassword }}
        <label for="verification">使用者輸入的驗證碼:</label>
        {{ verification }}
      </div>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    data() {
      return {
        memberPhoneNumber: "",
        memberPassword: "",
        verification: '',
        captchaImage: ''
      };
    },
    methods: {
      login() {
        // 創建包含請求數據的對象
        const requestData = {
          memberPhoneNumber: this.memberPhoneNumber,
          memberPassword: this.memberPassword,
          verification: this.verification
        };
        const jsonData = JSON.stringify(requestData);

  
        // 提交登入請求到後端，設置內容類型為 JSON
        axios.post('http://localhost:8081/esunlibrary/member/login', jsonData)
          .then(response => {
            // 登入成功，根據後端回應執行相應操作
            // 例如重定向到登入後的頁面
            // 或顯示登入成功的消息
            console.log(response.data + ' 登入成功');
          })
          .catch(error => {
            // 登入失敗，處理錯誤情況
            console.error(error);
          });
      },
      refreshCaptcha() {
        axios
          .get('http://localhost:8081/esunlibrary/member/generateImage', { responseType: 'arraybuffer' })
          .then(response => {
            const blob = new Blob([response.data], { type: 'image/png' });
            const imageUrl = URL.createObjectURL(blob);
            this.captchaImage = imageUrl;
          })
          .catch(error => {
            console.error(error);
          });
      }
    },
    mounted() {
      this.refreshCaptcha();
    }
  };
  </script>
  