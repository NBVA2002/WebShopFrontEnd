<template>
  <!-- <div >
    <h1>This is admin view </h1>
    <button type="submit" class="btn btn-dark" @click="logout">Logout</button>
  </div> -->
  <div class="admin-container">
    <h1 class="admin-heading">Trang quản lý sản phẩm</h1>
    <div class="product-form">
      <h2 class="form-heading">{{ editingIndex === null ? 'Thêm sản phẩm mới' : 'Sửa sản phẩm' }}</h2>
      <form @submit.prevent="editingIndex === null ? addProduct : updateProduct" class="form">
        <div class="form-group">
          <label for="product_name">Tên sản phẩm:</label>
          <input type="text" id="product_name" v-model="newProduct.name" class="form-input" required>
        </div>
        <div class="form-group">
          <label for="product_date">Ngày đăng lên:</label>
          <input type="date" id="product_date" v-model="newProduct.date" class="form-input" required>
        </div>
        <div class="form-group">
          <label for="product_details">Chi tiết sản phẩm:</label>
          <textarea id="product_details" v-model="newProduct.details" class="form-input" rows="4" required></textarea>
        </div>
        <div class="form-group">
          <label for="product_image">Ảnh sản phẩm:</label>
          <input type="file" id="product_image" ref="imageInput" @change="handleImageUpload" class="form-input" accept="image/*">
        </div>
        <button type="submit" class="button primary-button">{{ editingIndex === null ? 'Thêm sản phẩm' : 'Lưu chỉnh sửa' }}</button>
        <button v-if="editingIndex !== null" @click="cancelEdit" class="button secondary-button">Hủy</button>
      </form>
    </div>
    <div class="product-list">
      <h2 class="list-heading">Danh sách sản phẩm</h2>
      <ul>
        <li v-for="(product, index) in products" :key="index" class="product-item">
          <div class="product-info">
            <img :src="product.imageUrl" alt="Ảnh sản phẩm" class="product-image">
            <div class="product-details">
              <p class="product-name">{{ product.name }}</p>
              <p class="product-date">{{ formatDate(product.date) }}</p>
              <p class="product-details">{{ product.details }}</p>
            </div>
          </div>
          <div class="button-group">
            <button @click="editProduct(index)" class="button edit-button">Sửa</button>
            <button @click="deleteProduct(index)" class="button delete-button">Xóa</button>
          </div>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  // methods: {
  //   logout() {
  //     localStorage.removeItem("token");
  //     window.location.replace("http://localhost:8080/");
  //   },
  // }
  data() {
    return {
      products: [
        { name: "Sản phẩm 1", date: "2023-08-15", details: "Váy xinh xắn", imageUrl: "https://dvcfashion.com/wp-content/uploads/2023/07/vay-nau-phoi-beo-co-dinh-hoa-de-thuong-v3947.jpg" },
        { name: "Sản phẩm 2", date: "2023-08-16", details: "Chi tiết sản phẩm 2", imageUrl: "https://pos.nvncdn.net/4ef0bf-108661/art/artCT/20230414_ijKRDBHH.jpg" },
        // ... danh sách sản phẩm khác
      ],
      newProduct: { name: "", date: null, details: "", imageUrl: "" },
      editingIndex: null,
    }
  },
 
  methods: {
    addProduct() {
      if (this.newProduct.name && this.newProduct.date && this.newProduct.details && this.newProduct.imageUrl) {
        this.products.push({ ...this.newProduct });
        this.clearForm();
      }
    },
    editProduct(index) {
      this.editingIndex = index;
      this.newProduct = { ...this.products[index] };
    },
    updateProduct() {
      if (this.editingIndex !== null) {
        this.products[this.editingIndex] = { ...this.newProduct };
        this.cancelEdit();
      }
    },
    cancelEdit() {
      this.editingIndex = null;
      this.clearForm();
    },
    deleteProduct(index) {
      this.products.splice(index, 1);
    },
    clearForm() {
      this.newProduct = { name: "", date: null, details: "", imageUrl: "" };
    },
    formatDate(date) {
      const options = { year: 'numeric', month: 'short', day: 'numeric' };
      return new Date(date).toLocaleDateString(undefined, options);
    },
    handleImageUpload(event) {
      const file = event.target.files[0];
      if (file) {
        const reader = new FileReader();
        reader.onload = () => {
          this.newProduct.imageUrl = reader.result;
        };
        reader.readAsDataURL(file);
      }
    },
  },
}
</script>

<style scoped>
.admin-container {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
}

.admin-heading {
  font-size: 24px;
  margin-bottom: 20px;
}

.product-form {
  background-color: #f9f9f9;
  border-radius: 10px;
  padding: 20px;
  box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1);
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
  border-color: #3498db;
}

.button {
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  margin-top: 10px;
  width: fit-content;
}

.primary-button {
  background-color: #3498db;
  color: #fff;
}

.secondary-button {
  background-color: #e74c3c;
  color: #fff;
  margin-left: 10px;
}

.product-list {
  margin-top: 20px;
}

.list-heading {
  font-size: 20px;
  margin-bottom: 10px;
}

.product-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px 0;
  border-bottom: 1px solid #ccc;
}

.product-info {
  display: flex;
  align-items: center;
}

.product-image {
  width: 80px;
  height: 80px;
  object-fit: cover;
  border-radius: 5px;
  margin-right: 10px;
}

.product-details {
  flex-grow: 1;
}

.product-name {
  font-weight: bold;
  margin-bottom: 5px;
}

.product-date {
  color: #888;
  font-size: 14px;
}

.product-details {
  font-size: 14px;
}

.button-group {
  display: flex;
  align-items: center;
}

.edit-button,
.delete-button {
  padding: 6px 10px;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.edit-button {
  background-color: #3498db;
  color: #fff;
}

.delete-button {
  background-color: #e74c3c;
  color: #fff;
  margin-left: 5px;
}

@media (max-width: 600px) {
  .admin-container {
    padding: 10px;
  }

  .product-list {
    margin-top: 10px;
  }

  .product-image {
    width: 60px;
    height: 60px;
  }

  .form-input {
    padding: 8px;
  }
}
</style>
