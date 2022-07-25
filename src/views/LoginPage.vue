<template>
  <div>
    <form class="row g-3 my-1 m-0">
      <div class="col-md-4 offset-md-4 card border">
        <div class="login_page p-3">
          <div class="col-md-12">
            <label for="inputUsernameLogin" class="form-label">Kullanıcı Adı</label>
            <input
              type="text"
              class="form-control"
              id="inputUsernameLogin"
              v-model="userData.username"
            />
          </div>
          <div class="col-md-12">
            <label for="inputPasswordLogin" class="form-label">Password</label>
            <input
              type="password"
              class="form-control"
              id="inputPasswordLogin"
              v-model="userData.passwordContent"
            />
          </div>
          <div class="col-md-12 mt-3">
            <button type="submit" class="btn btn-primary" @click="doLogin">
              Giriş yap
            </button>
          </div>
          <div class="col-md-12 mt-3 d-flex justify-content-end">
            <p>
              hesabınız yok mu!
              <router-link
                class="btn btn-sm btn-secondary me-2"
                :to="{ name: 'RegisterPage' }"
              >
                Kayıtol</router-link
              >
            </p>
          </div>
        </div>
      </div>
    </form>
  </div>
</template>

<script>
import CryptoJs from "crypto-js";

export default {
  data() {
    return {
      userData: {
        username: null,
        passwordContent: null,
      },
    };
  },
  methods: {
    doLogin() {
      const password = CryptoJs.HmacSHA1(
        this.userData.passwordContent,
        this.$store.getters._saltKey
      ).toString();

      this.$appAxios
        .get(
          `/users?userName=${this.userData.username}&passwordFirst=${password}`
        )
        .then((login_response) => {
          // console.log(login_response);
         if(login_response?.data?.length > 0){
          this.$store.commit("setUser", login_response?.data[0]);
          this.$router.push({name : 'HomePage'});
         }else{
          alert("kullanıcı bulunamadı")
         }
          
        })
        .catch((err) => {
          console.log(err);
        });
      // console.log(this.userData);
    },
  },
};
</script>

<style scoped>
/* /////////giris ekranı//////// */

.login_form {
  background-color: #fff;
  border: 2px solid rgb(126, 121, 121);
  border-radius: 5px;
  display: flex;
  justify-content: center;
  height: auto;
  width: 300px;
  position: absolute;
  top: 52px;
  right: 20px;
}

/* /////////footer///////// */
</style>
