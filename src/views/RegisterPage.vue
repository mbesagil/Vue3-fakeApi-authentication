<template>
  <div>
    <form class="row g-3 my-1 m-0">
      <div class="col-md-4 offset-md-4 card border p-5">
        <div class="singin_page p-3">
          <div class="col-md-12">
            <label for="username" class="form-label">Kullanıcı Adı</label>
            <input
              type="text"
              v-model="userData.userName"
              class="form-control"
              id="username"
            />
          </div>
          <div class="col-md-12">
            <label for="name" class="form-label">Adı</label>
            <input
              type="text"
              v-model="userData.name"
              class="form-control"
              id="name"
            />
          </div>
          <!-- <div class="col-md-12">
            <label for="lastname" class="form-label">Soyadı</label>
            <input
              type="text"
              v-model="userData.lastName"
              class="form-control"
              id="lastname"
            />
          </div>
          <div class="col-md-12">
            <label for="inputEmailSingin" class="form-label">Email</label>
            <input
              type="email"
              class="form-control"
              v-model="userData.email"
              id="inputEmailSingin"
            />
          </div> -->
          <div class="password_group">
            <div class="col-md-12">
              <label for="inputPasswordSingin1" class="form-label">Şifre</label>
              <input
                type="password"
                class="form-control"
                v-model="userData.passwordFirst"
                id="inputPasswordSingin1"
              />
            </div>
            <!-- <div class="col-md-12">
              <label for="inputPasswordSingin2" class="form-label"
                >Şifre Onayı</label
              >
              <input
                type="password"
                class="form-control"
                v-model="userData.passwordVerify"
                id="inputPasswordSingin2"
              />
            </div> -->
          </div>
          <div class="col-md-12 mt-3">
            <button
              type="submit"
              class="btn btn-primary"
              @click="registerSaveData"
            >
              Kayıt ol
            </button>
          </div>
          <div class="col-md-12 mt-3 d-flex justify-content-end">
            <p>
              zaten bi hesabım var
              <router-link
                class="btn btn-sm btn-secondary me-2"
                :to="{ name: 'LoginPage' }"
              >
                Giriş Yap</router-link
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
        userName: null,
        name: null,
        // lastName: null,
        // email: null,
        passwordFirst: null,
        // passwordVerify: null,
      },
    };
  },
  methods: {
    registerSaveData() {
      const password = CryptoJs.HmacSHA1(
        this.userData.passwordFirst,
        this.$store.getters._saltKey
      ).toString();
      this.$appAxios
        .post("/users", { ... this.userData, passwordFirst : password })
        .then((used_response) => {
          console.log(used_response);
          this.$router.push({name : 'HomePage'});
        });
      // console.log(password);
    },
  },
};
</script>

<style>
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
</style>
