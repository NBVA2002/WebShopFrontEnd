<template>
  <!-- <div class="about">
    <h1>This is user view</h1>
    <button type="submit" class="btn btn-dark" @click="logout">Logout</button>
    <button type="submit" class="btn btn-dark" @click="evaluate">cmt</button>
  </div> -->
  <div class="user-container">
  <hr>
    <h1 class="user-heading">Tài khoản của tôi</h1>
    <div class="user-info">
    <hr>
      <table class="user-details">
        <tr>
          <th>Tên người dùng:</th>
          <td>{{ username }}</td>
        </tr>
        <tr>
          <th>Email:</th>
          <td>{{ email }}</td>
        </tr>
        <tr>
          <th>Số điện thoại:</th>
          <td>{{ phoneNumber }}</td>
        </tr>
        <tr>
          <th>Địa chỉ:</th>
          <td>{{ address }}</td>
        </tr>
        <tr>
          <th>Giới tính:</th>
          <td>{{ gender }}</td>
        </tr>
      </table>
      <div>
        <button @click="editUserInfo" class="button edit-button">Chỉnh sửa thông tin</button>
        <button @click="changePasswordForm" class="button edit-button">Thay đổi mật khẩu</button>
      </div>
    </div>
    <div v-if="editing" class="edit-form">
      <h2 class="form-heading">Chỉnh sửa thông tin</h2>
      <form @submit.prevent="saveUserInfo" class="form">
        <div class="form-group">
          <label for="edit_username">Tên người dùng:</label>
          <input type="text" id="edit_username" v-model="editedUsername" class="form-input" required>
        </div>
        <div class="form-group">
          <label for="edit_email">Email:</label>
          <input type="email" id="edit_email" v-model="editedEmail" class="form-input" required>
        </div>
        <div class="form-group">
          <label for="edit_phone">Số điện thoại:</label>
          <input type="text" id="edit_phone" v-model="editedPhoneNumber" class="form-input" required>
        </div>
        <div class="form-group">
          <label for="edit_address">Địa chỉ:</label>
          <input type="text" id="edit_address" v-model="editedAddress" class="form-input" required>
        </div>
        
        <div class="form-group">
          <label for="edit_gender">Giới tính:</label>
          <select id="edit_gender" v-model="editedGender" class="form-input" required>
            <option value="Nam">Nam</option>
            <option value="Nữ">Nữ</option>
            <option value="Khác">Khác</option>
          </select>
        </div>
        <button type="submit" class="button primary-button">Lưu thay đổi</button>
        <button @click="cancelEdit" class="button secondary-button">Hủy</button>
      </form>
    </div>
    <div v-if="changingPassword" class="password-form">
      <h2 class="form-heading">Thay đổi mật khẩu</h2>
      <form @submit.prevent="changePassword" class="form">
        <div class="form-group">
          <label for="current_password">Mật khẩu hiện tại:</label>
          <input type="password" id="current_password" v-model="currentPassword" class="form-input" required>
        </div>
        <div class="form-group">
          <label for="new_password">Mật khẩu mới:</label>
          <input type="password" id="new_password" v-model="newPassword" class="form-input" required>
        </div>
        <div class="form-group">
          <label for="confirm_password">Xác nhận mật khẩu mới:</label>
          <input type="password" id="confirm_password" v-model="confirmPassword" class="form-input" required>
        </div>
        <button type="submit" class="button primary-button">Thay đổi mật khẩu</button>
        <button @click="cancelChangePassword" class="button secondary-button">Hủy</button>
      </form>
    </div>
 
  </div>
</template>

<script>
// import axios from "axios";
export default {
//   methods: {
//     logout() {
//       localStorage.removeItem("token");
//       window.location.replace("http://localhost:8080/");
//     },
//     async evaluate() {
//       try {
//         const response = await axios.post(
//           "http://localhost:8081/evaluate/create/3",
//           {
//             rate: 4,
//             comment: "hehe",
//           },
//           {
//             headers: {
//               "Access-Control-Allow-Origin": "*",
//               Authorization: "Bearer " + localStorage.getItem("token"),
//             },
//           }
//         );
//         console.log(response);
//       } catch (error) {
//         console.error(error);
//       }
//     },
//   },
// };
data() {
    return {
      username: "Người dùng mẫu",
      email: "example@example.com",
      phoneNumber: "1234567890",
      address: "Địa chỉ mẫu",
      editedGender: "",
      editing: false,
      changingPassword: false,
      editedUsername: "",
      editedEmail: "",
      editedPhoneNumber: "",
      editedAddress: "",
      currentPassword: "",
      newPassword: "",
      confirmPassword: ""
    };
  },
  methods: {
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
      if (this.newPassword !== this.confirmPassword) {
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
    }
  }
};
</script>

<style scoped>

.user-container {
  max-width: 800px;
  margin: 0 auto;
  margin-top: 100px;
  padding: 20px;
}

.user-heading {
  font-size: 24px;
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

.primary-button {
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
  background-color: #e74c3c;
  color: #fff;
  padding: 8px 16px;
  border-radius: 5px;
  cursor: pointer;
}

.secondary-button:hover {
  background-color: #c0392b;
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
  background-color: #f2f2f2;
  text-align: left;
}

</style>

