<template>
  <div class="shopcart">
    <div class="grid">
      <div claSs="shopping-cart">
        <div class="list-item">
          <div
            class="item"
            v-for="cartegoryItem in shoppingCart"
            :key="cartegoryItem.product.id"
          >
            <img
              :src="
                'http://localhost:8081/file/' +
                cartegoryItem.product.imageEntities[0].imgURL
              "
              alt=""
              class="item-img"
            />
            <div class="item-product">
              {{ cartegoryItem.product.productName }}
              <h3>{{ formatPrice(cartegoryItem.product.price) }}</h3>
              <h3>Size: {{ cartegoryItem.size }}</h3>
              <h3>Số lượng: {{ cartegoryItem.quantity }}</h3>
            </div>
          </div>
        </div>
      </div>
      <div class="order-sumary">
        <h1>Order Summary</h1>
        <div class="total">
          <h3>Thành tiền: {{ formatPrice(amount) }}</h3>
          <button class="btn-order" @click="sendOrder">Đặt hàng</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      shoppingCart: [],
      orderId: 0,
      amount: 0,
    };
  },

  created() {
    if (
      JSON.parse(localStorage.getItem("shoppingCart")) == null ||
      JSON.parse(localStorage.getItem("shoppingCart")).length == 0
    ) {
      localStorage.setItem("shoppingCart", JSON.stringify(this.shoppingCart));
    } else {
      this.shoppingCart = JSON.parse(localStorage.getItem("shoppingCart"));
      this.shoppingCart.reverse();
      console.log(this.shoppingCart.length);
      this.shoppingCart.forEach((cartItem) => {
        this.amount += cartItem.product.price * cartItem.quantity;
      });
    }
  },

  getAmount() {},

  methods: {
    formatPrice(number) {
      return new Intl.NumberFormat("vi-VN", {
        style: "currency",
        currency: "VND",
      }).format(number);
    },

    addCart(product) {
      this.shoppingCart.push(product);
      localStorage.setItem("shoppingCart", JSON.stringify(this.shoppingCart));
      console.log(localStorage.getItem("shoppingCart"));
    },

    async sendOrder() {
      this.orderId = await this.createOder()
      console.log(this.orderId);
      for (var i = 0; i < this.shoppingCart.length; i++) {
        this.addCartItemOder(this.shoppingCart[i].product.id, this.shoppingCart[i].size, this.shoppingCart[i].quantity);
        console.log(this.shoppingCart[i].product.id);
      }
      localStorage.removeItem("shoppingCart");
      alert("Đặt hàng thành công");
      this.$router.push("/");
    },

    async createOder() {
      try {
        const response = await axios.post(
          "http://localhost:8081/order/create",
          {},
          {
            headers: {
              "Access-Control-Allow-Origin": "*",
              Authorization: "Bearer " + localStorage.getItem("token"),
            },
          }
        );
        return response.data.id;
      } catch (error) {
        console.error(error);
      }
    },

    async addCartItemOder(pid, size, quantity) {
      try {
        const response = await axios.post(
          "http://localhost:8081/cart/create/" + this.orderId + "?pid=" + pid,
          {
            size: size,
            quantity: quantity,
          },
          {
            headers: {
              "Access-Control-Allow-Origin": "*",
              Authorization: "Bearer " + localStorage.getItem("token"),
            },
          }
        );
          return response.data.id;
      } catch (error) {
        console.error(error);
      }
    },
  },
};
</script>

<style scoped>
.shopcart {
  margin-top: 100px;
}

.shopping-cart {
  width: 850px;
  height: 600px;
  display: flex;
  margin-top: 20px;
  margin-bottom: 20px;
  background-color: #c0c0c0;
}

.order-sumary {
  width: 500px;
  text-align: left;
}

.total {
  height: 200px;
  display: flex;
  justify-content: space-between;
  margin-top: 20px;
  margin-bottom: 20px;
  background-color: #c0c0c0;
}

.list-item {
  width: 100%;
  height: 100%;
  background-color: #c0c0c0;
  overflow: auto;
  overflow-y: scroll;
  padding: 20px;
}

.item {
  width: 100%;
  height: 150px;
  background-color: #fff;
  padding: 20px;
  border: 2px solid #000;
  border-radius: 20px;
  box-sizing: border-box;
  box-shadow: 0 0 10px #c0c0c0;
  display: flex;
}

.item img {
  width: 100px;
  object-fit: cover;
  margin-left: 20px;
  border-radius: 20px;
}

.item-product {
  display: inline-block;
  margin-left: 30px;
}

.grid {
  width: 100%;
  max-width: 1440px;
  max-height: 100%;
  margin: 0 auto;
  flex-shrink: 0;
  display: flex;
  justify-content: space-between;
}
</style>
