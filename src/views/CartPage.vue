<script setup>
import { cartStore } from "@/stores/Cart";
import { storeToRefs } from "pinia";
import { ref } from "vue";
import { computed } from "vue";
import { useRouter } from "vue-router";

const store = cartStore();
const { cartItems } = storeToRefs(store);
const router = useRouter();

const countries = ref(["Egypt", "Spain", "Germany", "France"]);

const calcToPrice = computed(() => {
  let total = 0;

  cartItems.value.forEach((item) => {
    total +=
      Math.ceil(item.price - item.price * (item.discountPercentage / 100)) *
      item.quantity;
  });
  return total;
});

const toCheckout = () => {
  store.setLocalStorage();
  router.push({ name: "checkout" });
};
</script>
<template>
  <div class="cart_page">
    <v-container fluid>
      <v-row>
        <v-col cols="12" class="pb-0">
          <v-breadcrumbs
            :items="['Home', 'Your Cart']"
            style="font-size: 13px"
            class="px-0"
          >
            <template v-slot:divider>
              <v-icon color="#878484">mdi-chevron-right</v-icon>
            </template>
          </v-breadcrumbs>
          <div class="text-center w-50 ma-auto" v-if="!cartItems.length">
            <img src="@/assets/images/bag.png" alt="" class="w-33" />
            <v-card-text class="text-grey-darken-1 pt-0 px-0"
              >Free Shipping For All Orders Over $10000.00</v-card-text
            >
            <v-card-text class="text-grey-darken-1 text-center"
              >Your shopping cart is empty</v-card-text
            >
            <v-actions>
              <v-btn
                color="#7399db"
                variant="elevated"
                class="text-capitalize mt-3 w-33 border"
                @click="router.push({ name: 'home' })"
                :class="{ 'rounded-pill': $vuetify.display.smAndUp }"
                >continue shopping</v-btn
              >
            </v-actions>
          </div>
        </v-col>
        <v-col cols="12" v-if="cartItems.length">
          <h1 class="pt-0 mb-8">Your Cart</h1>
          <div class="bar-progress position-relative">
            <v-progress-linear
              :color="
                parseInt((calcToPrice / 10000) * 100) <= 50
                  ? 'red'
                  : parseInt((calcToPrice / 10000) * 100) > 50 &&
                    parseInt((calcToPrice / 10000) * 100) < 100
                  ? 'orange'
                  : 'green'
              "
              height="10"
              striped
              :model-value="
                parseInt((calcToPrice / 10000) * 100) <= 100
                  ? parseInt((calcToPrice / 10000) * 100)
                  : 100
              "
            ></v-progress-linear>
            <svg
              class="icon-shipping-truck"
              viewBox="0 0 40.55 24"
              :fill="
                parseInt((calcToPrice / 10000) * 100) <= 50
                  ? 'red'
                  : parseInt((calcToPrice / 10000) * 100) > 50 &&
                    parseInt((calcToPrice / 10000) * 100) < 100
                  ? 'orange'
                  : 'green'
              "
              width="40.55"
              :style="` position: absolute;
                bottom: 50%;
                left: calc(${
                  parseInt((calcToPrice / 10000) * 100) <= 100
                    ? parseInt((calcToPrice / 10000) * 100)
                    : 100
                }% - 40.55px);
                transition: all 0.15s ease-in-out;`"
            >
              <g id="Layer_2" data-name="Layer 2">
                <g id="Layer_1-2" data-name="Layer 1">
                  <path
                    class="truck-body"
                    d="M40.43,11a3.86,3.86,0,0,0-3.68-2.65H28a1.25,1.25,0,0,1-1.43-1.43c0-2.18,0-4.35,0-6.53,0-.31-.08-.36-.37-.36H5.11a1.18,1.18,0,0,0-1.3,1.32c0,.74,0,1.48,0,2.22,0,.21-.06.27-.26.26-.36,0-.71,0-1.07,0a1.19,1.19,0,1,0,0,2.37H7.19c.43,0,.85,0,1.27,0a1,1,0,0,1,1.07,1A1.19,1.19,0,0,1,8.24,8.48H1.35a1.83,1.83,0,0,0-.47,0A1.19,1.19,0,0,0,0,9.85a1.18,1.18,0,0,0,1.19,1h9.66c.34,0,.68,0,1,0A1.19,1.19,0,0,1,13,12.47a1.26,1.26,0,0,1-1.26.76H1.24a1.19,1.19,0,1,0,0,2.38c.76,0,1.51,0,2.26,0,.26,0,.33.07.32.32,0,1,0,2.09,0,3.13A1.18,1.18,0,0,0,5.1,20.36c.63,0,1.26,0,1.9,0,.27,0,.39.06.47.36a4.55,4.55,0,0,0,8.78-.11.29.29,0,0,1,.32-.25H28.09a.3.3,0,0,1,.34.27,4.55,4.55,0,0,0,8.8,0,.31.31,0,0,1,.35-.26c.49,0,1,0,1.47,0a1.37,1.37,0,0,0,1.5-.87V11.41C40.41,11.29,40.47,11.12,40.43,11ZM32.84,21.62A2.18,2.18,0,1,1,35,19.45,2.21,2.21,0,0,1,32.84,21.62Zm-21,0A2.18,2.18,0,1,1,14,19.45,2.2,2.2,0,0,1,11.86,21.62Z"
                  ></path>
                  <path
                    class="truck-body"
                    d="M29.27,6h5.85c.1,0,.2,0,.29,0C33.64,2.72,32,.91,28.91.26V.57c0,1.68,0,3.35,0,5C28.9,5.9,29,6,29.27,6Z"
                  ></path>
                  <path
                    class="wheel"
                    fill="#fff"
                    d="M11.87,17.27A2.18,2.18,0,1,0,14,19.45,2.2,2.2,0,0,0,11.87,17.27Z"
                  ></path>
                  <path
                    class="wheel"
                    fill="#fff"
                    d="M32.85,17.27A2.18,2.18,0,1,0,35,19.45,2.22,2.22,0,0,0,32.85,17.27Z"
                  ></path>
                </g>
              </g>
            </svg>
          </div>
          <v-card-text
            class="text-grey-darken-1 px-0 pb-0"
            v-if="cartItems.length && 10000 - calcToPrice > 0"
            >Only ${{ 10000 - calcToPrice }} Away From Free
            Shipping</v-card-text
          >
          <v-card-text
            class="text-green-darken-1 px-0 pb-0"
            v-if="cartItems.length && 10000 - calcToPrice <= 0"
            >Congrats on Free Shipping</v-card-text
          >
        </v-col>
        <v-col cols="12" md="8" v-if="cartItems.length">
          <v-table hover class="mt-5 w-100 border-b-md">
            <thead class="bg-grey">
              <tr>
                <th style="width: 50%" class="text-left font-weight-bold ps-3">
                  PRODUCT
                </th>
                <th
                  style="width: 12%"
                  class="text-left font-weight-bold text-center"
                >
                  PRICE
                </th>
                <th
                  style="width: 12%"
                  class="text-left font-weight-bold text-center"
                >
                  QUANTITY
                </th>
                <th
                  style="width: 12%"
                  class="text-left font-weight-bold text-center"
                >
                  TOTAL
                </th>
                <th
                  style="width: 12%"
                  class="text-left font-weight-bold text-center"
                >
                  REMOVE
                </th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="item in cartItems" :key="item.id">
                <td class="">
                  <v-row>
                    <v-col cols="12" md="4">
                      <img :src="item.thumbnail" class="w-100" alt="" />
                    </v-col>
                    <v-col cols="12" md="7">
                      <v-card-title
                        style="
                          white-space: pre-wrap;
                          font-size: 14px;
                          line-height: 1.2;
                        "
                        class="px-0"
                        >{{ item.title }} Sample - ({{
                          item.category
                        }})</v-card-title
                      >
                      <v-card-text class="text-grey-darken-1 px-0">
                        Category: {{ item.category }}
                      </v-card-text>
                    </v-col>
                  </v-row>
                </td>
                <td>${{ item.price }}</td>
                <td>
                  <div
                    class="quantity rounded-pill border px-3 py-1 d-flex align-center"
                    style="width: fit-content"
                  >
                    <v-icon
                      class="cursor-pointer"
                      @click="item.quantity > 1 ? item.quantity-- : false"
                      >mdi-minus</v-icon
                    >
                    <input
                      type="number"
                      v-model="item.quantity"
                      min="1"
                      max="10"
                      class="text-center text-body-1"
                    />
                    <v-icon class="cursor-pointer" @click="item.quantity++"
                      >mdi-plus</v-icon
                    >
                  </div>
                </td>
                <td>
                  <v-card-text class="px-0 py-0 font-weight-bold text-center">
                    ${{ item.price * item.quantity }}
                  </v-card-text>
                </td>
                <td class="text-center">
                  <v-icon
                    class="cursor-pointer text-red"
                    @click="store.removeItem(item)"
                    >mdi-close</v-icon
                  >
                </td>
              </tr>
            </tbody>
          </v-table>
          <div class="mt-8 d-flex align-center justify-lg-space-between">
            <div>
              <v-card-text
                class="text-grey-darken-1 px-0 pb-0 align-center d-flex ga-2"
                v-if="cartItems.length"
              >
                <span>
                  <svg
                    id="Layer_1"
                    data-name="Layer 1"
                    width="20"
                    xmlns="http://www.w3.org/2000/svg"
                    viewBox="0 0 179.94 179.96"
                    class="icon icon-shield-check"
                  >
                    <path
                      d="M90,0,5,42.78C13.73,105.26,38.14,154.32,90,180c51.83-25.64,76.25-74.7,85-137.18Z"
                    ></path>
                    <polygon
                      fill="#fff"
                      class="cls-1"
                      points="149.83 67.57 134.81 52.55 79.31 108.05 49.74 78.48 34.72 93.5 79.15 137.94 79.31 137.78 79.47 137.94 149.83 67.57"
                    ></polygon>
                  </svg>
                </span>
                Secure Shopping Guarantee</v-card-text
              >
              <img
                src="@/assets/images/cart-page-cards.webp"
                alt=""
                class="w-25 mt-4"
              />
            </div>
            <div>
              <v-btn
                class="mt-4 rounded-pill text-capitalize"
                color="red"
                @click="store.clearCart()"
                >Clear Cart</v-btn
              >
            </div>
          </div>
        </v-col>
        <v-col
          cols="12"
          md="4"
          class="pa-5 mt-8 mt-md-0"
          v-if="cartItems.length"
        >
          <v-card elevation="0">
            <v-card-title
              style="font-size: 15px"
              class="font-weight-bold bor border-b-lg mb-4"
              >ORDER SUMMARY</v-card-title
            >
            <v-card-text class="border-b-sm">
              <span>Subtotal</span>
              <span class="float-right font-weight-black"
                >${{ calcToPrice }}</span
              >
            </v-card-text>
            <v-card-text class="text-grey-darken-1">
              Get Shipping Estimate
              <select class="w-100 border pa-3 rounded-pill mt-2">
                <option v-for="country in countries" :key="country" value="">
                  {{ country }}
                </option>
              </select>
              <div class="sautes">
                <select class="w-50 border pa-3 rounded-pill mt-2">
                  <option v-for="country in countries" :key="country" value="">
                    {{ country }}
                  </option>
                </select>
                <input
                  type="text"
                  placeholder="postal code"
                  class="w-33 border pa-3 rounded-pill mt-2 ms-16"
                />
              </div>
            </v-card-text>
            <v-card-actions>
              <v-btn
                variant="outlined"
                density="compact"
                color="white"
                height="40"
                class="text-capitalize w-100 rounded-pill bg-blue-darken-4 mt-3 font-weight-bold border"
              >
                Calculate Shipping
              </v-btn>
            </v-card-actions>
            <v-card-text class="border-t-sm mt-2 border-b-sm">
              <span>Total</span>
              <span class="float-right font-weight-black"
                >${{ calcToPrice }}</span
              >
            </v-card-text>
            <v-card-actions class="flex-column">
              <v-btn
                variant="outlined"
                density="compact"
                color="white"
                height="40"
                @click="toCheckout"
                class="text-capitalize w-100 bg-blue-darken-3 rounded-0 mt-3 font-weight-bold border"
              >
                Proceed to Checkout
              </v-btn>
              <v-btn
                variant="outlined"
                density="compact"
                color="#999999"
                height="40"
                class="text-capitalize w-100 rounded-0 mt-3 font-weight-bold border"
                @click="router.push({ name: 'home' })"
              >
                Continue Shopping
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>

<style scoped lang="scss">
@media (max-width: 767px) {
  .cart_page {
    table {
      width: 800px !important;
    }
  }
}
</style>
