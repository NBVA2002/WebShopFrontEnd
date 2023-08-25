<template>
  <div class="user-container">
    <hr />
    <h1 class="user-heading">Tài khoản của tôi</h1>
    <div class="user-info">
      <hr />
      <table class="user-details">
        <tr>
          <th>Tên người dùng:</th>
          <td>{{ user.firstName }} {{ user.lastName }}</td>
        </tr>
        <tr>
          <th>Email:</th>
          <td>{{ user.email }}</td>
        </tr>
        <tr>
          <th>Số điện thoại:</th>
          <td>{{ user.phone }}</td>
        </tr>
        <tr>
          <th>Địa chỉ:</th>
          <td>{{ user.address }}</td>
        </tr>
      </table>
      <div>
        <button @click="editUserInfo" class="button edit-button">
          Chỉnh sửa thông tin
        </button>
        <button @click="changePasswordForm" class="button edit-button">
          Thay đổi mật khẩu
        </button>
        <button @click="getOrder" class="button edit-button">
          Xem đơn hàng
        </button>
      </div>
    </div>
    <div v-if="editing" class="edit-form">
      <h2 class="form-heading">Chỉnh sửa thông tin</h2>
      <form @submit.prevent="saveUserInfo" class="form">
        <div class="form-group">
          <label for="edit_username">Tên người dùng:</label>
          <input
            type="text"
            id="edit_username"
            v-model="user.username"
            class="form-input"
            required
          />
        </div>
        <div class="form-group">
          <label for="edit_email">Email:</label>
          <input
            type="email"
            id="edit_email"
            v-model="user.email"
            class="form-input"
            required
          />
        </div>
        <div class="form-group">
          <label for="edit_phone">Số điện thoại:</label>
          <input
            type="text"
            id="edit_phone"
            v-model="user.phone"
            class="form-input"
            required
          />
        </div>
        <div class="form-group">
          <label for="edit_address">Địa chỉ:</label>
          <input
            type="text"
            id="edit_address"
            v-model="user.address"
            class="form-input"
            required
          />
        </div>
        <div class="display-btn">
          <button type="submit" class="button primary-button">
            Lưu thay đổi
          </button>
          <button @click="cancelEdit" class="button secondary-button">
            Hủy
          </button>
        </div>
      </form>
    </div>
    <div v-if="changingPassword" class="password-form">
      <h2 class="form-heading">Thay đổi mật khẩu</h2>
      <form @submit.prevent="changePassword" class="form">
        <div class="form-group">
          <label for="current_password">Mật khẩu hiện tại:</label>
          <input
            type="password"
            id="current_password"
            v-model="currentPassword"
            class="form-input"
            required
          />
        </div>
        <div class="form-group">
          <label for="new_password">Mật khẩu mới:</label>
          <input
            type="password"
            id="new_password"
            v-model="user.password"
            class="form-input"
            required
          />
        </div>
        <div class="form-group">
          <label for="confirm_password">Xác nhận mật khẩu mới:</label>
          <input
            type="password"
            id="confirm_password"
            v-model="confirmPassword"
            class="form-input"
            required
          />
        </div>
        <div class="display-btn">
          <button type="submit" class="button primary-button">
            Thay đổi mật khẩu
          </button>
          <button @click="cancelChangePassword" class="button secondary-button">
            Hủy
          </button>
        </div>
      </form>
    </div>

    <div v-if="showOrder" class="order">
      <div class="list-order">
        <h2>Tổng số đơn hàng: {{ order.totalElements }}</h2>
        <div
          class="order-item"
          v-for="orderItem in order.content"
          :key="orderItem.id"
        >
          <div class="display-item underline">
            <h4>Mã đơn hàng: {{ orderItem.id }}</h4>
            <h4>Tổng tiền: {{ formatPrice(orderItem.totalPrice) }}</h4>
            <h4>Ngày đặt hàng: {{ orderItem.orderDate }}</h4>
            <h4>Tình trạng: {{ getStatus(orderItem.hasProcess) }}</h4>
          </div>

          <div class="display-item">
            <h4 style="width: 120px"></h4>
            <h4 style="width: 235px">Tên sản phẩm</h4>
            <h4 style="width: 50px">Size</h4>
            <h4 style="width: 100px">Số lượng</h4>
            <h4 style="width: 90px">Giá tiền</h4>
            <h4 style="width: 100px">Thành tiền</h4>
            <h4></h4>
          </div>

          <div class="display-item">
            <div
              class="cart-item"
              v-for="(cartItem, index) in orderItem.cartItemEntity"
              :key="index"
            >
              <img
                :src="
                  'http://localhost:8081/file/' +
                  cartItem.productEntity.imageEntities[0].imgURL
                "
                alt=""
                class="item-img"
              />
              <h2>{{ cartItem.productEntity.productName }}</h2>
              <h3 style="width: 30px">{{ cartItem.size }}</h3>
              <h3 style="width: 100px">{{ cartItem.quantity }}</h3>
              <h3>{{ formatPrice(cartItem.productEntity.price) }}</h3>
              <h3>
                {{
                  formatPrice(cartItem.productEntity.price * cartItem.quantity)
                }}
              </h3>
              <h3></h3>
            </div>
          </div>
        </div>
      </div>
      <div class="v-pagination__container">
        <div class="pagination__left">
          <button @click="cancelShowOrder" class="button secondary-button">
            Đóng
          </button>
        </div>
        <div class="pagination__right">
          <div class="pagination">
            <div class="pagination__prev" @click="changePage('prev')">&lt;</div>
            <div
              v-for="(page, index) in pages"
              :key="index"
              class="pagination__item"
              :class="{ 'pagination__item--active': page == pageNumber }"
              @click="changePage(page)"
            >
              {{ page }}
            </div>
            <div class="pagination__after" @click="changePage('after')">></div>
          </div>
        </div>
      </div>
    </div>
    <div style="display: flex;justify-content: space-between;">
      <div></div>
    <button class="logout" @click="logout">Đăng xuất <font-awesome-icon :icon="['fas', 'right-from-bracket']" /></button>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      username: "Người dùng mẫu",
      email: "example@example.com",
      phoneNumber: "1234567890",
      address: "Địa chỉ mẫu",
      editedGender: "",
      editing: false,
      changingPassword: false,
      showOrder: false,
      editedUsername: "",
      editedEmail: "",
      editedPhoneNumber: "",
      editedAddress: "",
      currentPassword: "",
      newPassword: "",
      confirmPassword: "",
      user: {},
      order: [],
      pageNumber: 1,
      totalPage: 0,
    };
  },

  props: ["islogin", "isAdmin"],

  methods: {
    logout() {
      localStorage.removeItem("token");
      window.location.replace("http://localhost:8080/login");
    },

    formatPrice(number) {
      return new Intl.NumberFormat("vi-VN", {
        style: "currency",
        currency: "VND",
      }).format(number);
    },

    editUserInfo() {
      this.editing = true;
      this.editedUsername = this.username;
      this.editedEmail = this.email;
      this.editedPhoneNumber = this.phoneNumber;
      this.editedAddress = this.address;
      this.editedBirthDate = this.birthDate;
      this.editedGender = this.gender;
    },

    saveUserInfo() {
      this.username = this.editedUsername;
      this.email = this.editedEmail;
      this.phoneNumber = this.editedPhoneNumber;
      this.address = this.editedAddress;
      this.gender = this.editedGender;
      this.editing = false;
    },

    cancelEdit() {
      this.editing = false;
    },

    changePasswordForm() {
      this.changingPassword = true;
    },

    changePassword() {
      if (this.user.password !== this.confirmPassword) {
        alert("Mật khẩu mới và xác nhận mật khẩu mới không khớp.");
        return;
      }

      // Sau khi thay đổi thành công:
      this.changingPassword = false;
      alert("Thay đổi mật khẩu thành công!");
      this.currentPassword = "";
      this.newPassword = "";
      this.confirmPassword = "";
    },

    cancelChangePassword() {
      this.changingPassword = false;
      this.currentPassword = "";
      this.newPassword = "";
      this.confirmPassword = "";
    },

    getOrder() {
      this.showOrder = true;
      this.getListOrder();
    },

    cancelShowOrder() {
      this.showOrder = false;
    },

    getStatus(status) {
      if (status == true) {
        return "Đã được xử lý";
      }
      return "Chưa được xử lý";
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

    async getListOrder() {
      try {
        const response = await axios.get(
          "http://localhost:8081/order/list/" +
            this.user.id +
            "?limit=1&page=" +
            this.pageNumber +
            "&sortby=id&sort=desc",
          {
            headers: {
              "Access-Control-Allow-Origin": "*",
              Authorization: "Bearer " + localStorage.getItem("token"),
            },
          }
        );
        this.order = response.data;
        this.totalPage = this.order.totalPages;
        return this.user;
      } catch (error) {
        console.error(error);
      }
    },

    changePage(page) {
      if (page == "prev" && this.pageNumber > 1) {
        this.pageNumber--;
      } else if (page == "after" && this.pageNumber < this.totalPage) {
        this.pageNumber++;
      } else if (typeof page == "number") {
        this.pageNumber = page;
      }
      // this.$emit("update:pageNumber", this.pageNumber);
      this.getListOrder();
    },
  },

  created() {
    if (!this.islogin) {
      this.$router.push("/login");
    } else {
      this.current();
    }
  },

  computed: {
    pages() {
      let pages = [];
      for (let i = 1; i <= this.totalPage; i++) {
        if (
          i == 1 ||
          i == this.totalPage ||
          Math.abs(i - this.pageNumber) <= 1
        ) {
          pages.push(i);
        } else if (Math.abs(i - this.pageNumber) == 3) {
          pages.push("...");
        }
      }
      return pages;
    },
  },
};
</script>

<style scoped>
.user-container {
  max-width: 1000px;
  margin: 0 auto;
  margin-top: 100px;
  padding: 20px;
}

.user-heading {
  font-size: 30px;
  margin-bottom: 20px;
}

.user-info {
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-bottom: 1px solid #ccc;
  padding-bottom: 20px;
  margin-bottom: 20px;
}

.user-details {
  flex-grow: 1;
}

.edit-button {
  padding: 8px 16px;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s ease;
  background-color: #42b983;
  color: #fff;
  margin: 15px;
}

.edit-button:hover {
  background-color: #42b983;
}

.edit-form,
.password-form {
  background-color: #f9f9f9;
  border-radius: 10px;
  padding: 20px;
  box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1);
  margin-top: 20px;
}

.form-heading {
  font-size: 20px;
  margin-bottom: 10px;
}

.form {
  display: flex;
  flex-direction: column;
}

.form-group {
  margin-bottom: 15px;
}

.form-input {
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
  width: 100%;
}

.form-input:focus {
  outline: none;
  border-color: #42b983;
}

.display-btn {
  display: flex;
  justify-content: space-around;
}

.primary-button {
  width: 300px;
  background-color: #42b983;
  color: #fff;
  padding: 8px 16px;
  border-radius: 5px;
  cursor: pointer;
  margin-top: 10px;
  margin-bottom: 10px;
}

.primary-button:hover {
  background-color: #42b983;
}

.secondary-button {
  width: 300px;
  background-color: #fff;
  color: #000;
  padding: 8px 16px;
  border-radius: 5px;
  cursor: pointer;
  margin-top: 10px;
  margin-bottom: 10px;
}

.secondary-button:hover {
  background-color: #42b983;
  color: #fff;
}

@media (max-width: 600px) {
  .user-container {
    padding: 10px;
  }

  .user-info {
    flex-direction: column;
    align-items: flex-start;
  }

  .edit-button,
  .secondary-button {
    margin-top: 10px;
  }

  .form-input {
    padding: 8px;
  }
}

.user-details {
  width: 100%;
  border-collapse: collapse;
  margin-bottom: 20px;
}

.user-details th,
.user-details td {
  padding: 8px 16px;
  border: 1px solid #ccc;
}

.user-details th {
  width: 200px;
  background-color: #f2f2f2;
  text-align: left;
}

.order {
  border: 1px solid #ccc;
  border-radius: 10px;
  padding: 20px;
  background-color: #fff;
  box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1);
  margin-top: 20px;
}

.list-order h2 {
  text-align: left;
  margin-left: 30px;
  margin-bottom: 25px;
}

.display-item {
  display: flex;
  justify-content: space-between;
  flex-wrap: wrap;
}

.underline {
  border-bottom: 1px solid #ccc;
  padding-bottom: 10px;
  margin-bottom: 10px;
}

.cart-item {
  width: 100%;
  height: 150px;
  /* background: #c0392b; */
  border-radius: 20px;
  margin-top: 5px;
  margin-bottom: 5px;
  display: flex;
  justify-content: space-between;
  border: 1px solid #ccc;
}

.cart-item h2 {
  line-height: 150px;
  margin: 0px;
}

.cart-item h3 {
  line-height: 150px;
}

.item-img {
  width: 120px;
  height: 150px;
  /* object-fit: cover; */
  border-radius: 20px;
  margin-right: 10px;
}

.v-pagination__container {
  width: 100%;
  margin-top: 25px;
  margin-bottom: 25px;
  display: flex;
  justify-content: space-around;
}

.pagination {
  display: flex;
}

.pagination__prev,
.pagination__after,
.pagination__item {
  width: 40px;
  height: 40px;
  padding-left: 10px;
  padding-right: 10px;
  line-height: 40px;
  border: 1px solid #c0c0c0;
  cursor: pointer;
}

.pagination__prev,
.pagination__after,
.pagination__item:hover {
  background-color: #c0c0c0;
}

.pagination__item--active {
  box-sizing: border-box;
  border: 3px solid #42b983;
  line-height: 36px;
}

.logout {
  padding: 8px 16px;
  border-radius: 5px;
  cursor: pointer;
  background-color: #fff;
  color: #000;
  margin: 15px;
}

.logout:hover {

  background-color: #42b983;
  color: #fff;
}

</style>

