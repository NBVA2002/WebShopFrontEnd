<template>
  <div class="nav-bar">
    <div class="grid">
      <div class="logo-place">
        <router-link to="/" class="logo-img">
          <img src="../assets/images/logo.png" alt="" />
        </router-link>
      </div>
      <nav>
        <router-link to="/">Home</router-link>
        <router-link to="/men">Nam</router-link>
        <router-link to="/women">Nữ</router-link>
        <router-link to="/accessories">Phụ kiện</router-link>
        <router-link to="/about">Bộ sưu tập</router-link>
        <router-link to="/shop">Shop</router-link>
        <router-link to="/about">About</router-link>
        <router-link to="/search" class="magnifying-glass">
          <font-awesome-icon :icon="['fas', 'magnifying-glass']" />
        </router-link>
        <router-link to="/about" class="shop-cart">
          <font-awesome-icon :icon="['fas', 'bell']" />
        </router-link>
        <div class="user">
          <router-link to="/login" v-if="!islogin">
            <font-awesome-icon :icon="['fas', 'user']" />
          </router-link>
          <router-link to="/user" v-if="islogin && !adminCheck()">
            <font-awesome-icon :icon="['fas', 'user']" />
          </router-link>
          <router-link to="/admin" v-if="islogin && adminCheck()">
            <font-awesome-icon :icon="['fas', 'user']" />
          </router-link>
        </div>
        <router-link to="/shopcart" class="shop-cart">
          <font-awesome-icon :icon="['fas', 'cart-shopping']" />
        </router-link>
      </nav>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      user: {},
    };
  },
  props: ["islogin"],

  methods: {
    login() {
      this.$router.push("/login");
    },
    adminCheck() {
      return this.user.roles == "ROLE_ADMIN";
    },
    async current() {
      try {
        const response = await axios.get("http://localhost:8081/api/current", {
          headers: {
            "Access-Control-Allow-Origin": "*",
            Authorization: "Bearer " + localStorage.getItem("token"),
          },
        });
        this.user = response.data.userEntity;
        return this.user;
      } catch (error) {
        console.error(error);
      }
    },
  },
  created() {
    if (this.islogin) {
      this.current();
    }
  },
};
</script>

<style scoped>
.nav-bar {
  background: #fff;
  box-shadow: 0px 4px 5px 0px rgba(0, 0, 0, 0.05);
  width: 100%;
  height: 70px;
  border-bottom: 1px solid #000;
}

.grid {
  width: 100%;
  height: 100%;
  max-width: 1440px;
  margin: 0 auto;
  flex-shrink: 0;
  display: flex;
  justify-content: space-between;
}

.logo-place {
  position: relative;
  margin-left: 15px;
}

.logo-place .logo-img {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
}

.nav-bar nav {
  height: 70px;
  padding: 0px;
}

.nav-bar nav a {
  font-size: 20px;
  margin-right: 20px;
  margin-left: 20px;
  text-decoration: none;
  line-height: 70px;
}

.nav-bar nav a:hover {
  color: #42b983;
  border-bottom: 3px solid #42b983;
}

.user {
  display: inline-block;
}

.magnifying-glass:hover {
  color: #42b983;
}

/* 

.user {
  width: 45px;
  height: 45px;
  margin-right: 20px;
  margin-left: 20px;
  background: #348b6b;
  border-radius: 25px;
  border: 2px solid #000;
  display: inline-block;
  box-sizing: border-box;
  cursor: pointer;
  transform: translateY(15px);
  position: relative;
}

.user:hover ul {
  display: block;
}

.test {
  width: 60px;
  height: 60px;
  transform: translateX(-2px);
}

.user ul {
  display: none;
  position: absolute;
  top: 53px;
  background: #fff;
  box-shadow: 0px 4px 10px 0px rgba(0, 0, 0, 0.05);
  width: 200px;
  list-style: none;
}

.user ul li {
  padding: 10px;
  padding-left: 20px;
  text-align: left;
  font-size: 18px;
  font-weight: 700;
  text-decoration: none;
  color: #000;
}

.user ul li a {
  text-decoration: none;
}

.user ul li:hover {
  background: #42b983;
} */
</style>
