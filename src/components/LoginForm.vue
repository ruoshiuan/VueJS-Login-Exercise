<template>
  <header>
    <h1>{{ slogan }}</h1>
  </header>
  <section>
    <form class="login" @submit.prevent v-if="isLogout">
      <h2>登入測試</h2>
      <input
        class="login__input"
        type="email"
        placeholder="帳號(信箱)"
        required
        v-model="user.account"
        @blur="validateAccount"
      />
      <input
        class="login__input"
        type="password"
        placeholder="密碼"
        required
        v-model="user.password"
        @blur="validatePassword"
      />
      <label class="login__warn" v-if="errors.account">
        <strong>帳號欄位必須是 email 格式</strong>
      </label>
      <label class="login__warn" v-if="errors.password">
        <strong>密碼欄位長度必須大於 8 個字元</strong>
      </label>
      <label class="login__warn" v-if="errors.loginFailedMsg">
        <strong>登入失敗</strong>
      </label>
      <button type="submit" class="login__btn btn" @click="loginOkFetch">
        登入 (Ok)
      </button>
      <button type="submit" class="login__btn btn" @click="loginFailedFetch">
        登入 (Failed)
      </button>
    </form>
    <h2 v-if="!isLogout">您好！{{ user.username }}</h2>
    <button class="logout__btn btn" v-if="!isLogout" @click="handleLogout">
      登出
    </button>
  </section>
</template>

<script>
const url = "https://2dbbbf75-5d79-41f5-b432-4a953eb0f52d.mock.pstmn.io/login";
function requestOptions(account, password) {
  return {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({
      account: account,
      password: password,
    }),
  };
}
function isVaildAccount(account) {
  const reg = /^\w+([\w.-]){1,63}@\w+([\w.-])\.\w+([\w.-])$/;
  return reg.test(account);
}
function isVaildPassword(password) {
  const reg = /.{8,}$/;
  return reg.test(password);
}
export default {
  name: "LoginForm",
  props: {
    slogan: String,
  },
  data() {
    return {
      isLogout: true,
      user: {
        account: "",
        password: "",
        username: "",
      },
      errors: {
        account: false,
        password: false,
        loginFailedMsg: false,
      },
    };
  },
  async created() {
    const isLogining = localStorage.getItem("token");
    if (isLogining) {
      this.isLogout = false;
    }
    const requestOptions = {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({
        token: isLogining,
      }),
    };
    try {
      const response = await fetch(`${url}/ok`, requestOptions);
      const data = await response.json();
      this.user.username = data.user.name;
    } catch (error) {
      console.log(error);
    }
  },
  methods: {
    validateAccount() {
      const isVaild = isVaildAccount(this.user.account);
      this.errors.account = !isVaild;
      this.errors.loginFailedMsg = false;
    },
    validatePassword() {
      const isVaild = isVaildPassword(this.user.password);
      this.errors.password = !isVaild;
      this.errors.loginFailedMsg = false;
    },
    async loginFailedFetch() {
      if (
        isVaildAccount(this.user.account) === true &&
        isVaildPassword(this.user.password) === true
      ) {
        try {
          const response = await fetch(
            `${url}/error`,
            requestOptions(this.user.account, this.user.password)
          );
          const data = await response.json();
          if (data) {
            this.errors.loginFailedMsg = true;
            this.user.password = "";
          }
        } catch (error) {
          console.log(error);
        }
      }
    },
    async loginOkFetch() {
      if (
        isVaildAccount(this.user.account) === true &&
        isVaildPassword(this.user.password) === true
      ) {
        try {
          const response = await fetch(
            `${url}/ok`,
            requestOptions(this.user.account, this.user.password)
          );
          const data = await response.json();
          if (data) {
            this.user.password = "";
            this.errors.loginFailedMsg = false;
            this.isLogout = false;
            this.user.username = data.user.name;
            localStorage.setItem("token", data.token);
          }
        } catch (error) {
          console.log(error);
        }
      }
    },
    handleLogout() {
      this.isLogout = true;
      localStorage.removeItem("token");
    },
  },
};
</script>

<style scoped>
section {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}
.login {
  width: 300px;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 10px;
  background-color: #eee;
  color: #2c3e50;
  border-radius: 2px;
  box-sizing: border-box;
}
.login__input {
  margin: 5px 0;
  width: 250px;
  border: 1px solid #ddd;
  height: 35px;
  border-radius: 2px;
  padding-left: 10px;
}
.login__warn {
  color: #f24;
  padding: 5px;
}
.btn {
  width: 250px;
  padding: 10px 0;
  cursor: pointer;
  border: none;
  border-radius: 2px;
  margin: 10px;
  font-weight: 500;
}
.login__btn {
  background-color: #2c3e50;
  color: #fff;
}
.logout__btn {
  background-color: #fff;
  color: #2c3e50;
}
@media screen and (max-width: 576px) {
  .login {
    min-width: 100%;
  }
}
</style>
