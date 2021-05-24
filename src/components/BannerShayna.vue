<template>
  <!-- Women Banner Section Begin -->
  <section class="women-banner spad">
    <div class="container-fluid">
      <div class="row">
        <div class="col-lg-12 mt-5" v-if="products.length > 0">
          <carousel
            class="product-slider"
            :items="3"
            :dots="false"
            :nav="false"
            :autoplay="true"
          >
            <div
              class="product-item"
              v-for="itemProduct in products"
              v-bind:key="itemProduct.id"
            >
              <div class="pi-pic">
                <img v-bind:src="itemProduct.galleries[0].photo" class="img-default" />
                <ul>
                  <li class="w-icon active">
                    <a
                        @click="
                          saveKeranjang(
                            itemProduct.id,
                            itemProduct.name,
                            itemProduct.price,
                            itemProduct.galleries[0].photo
                          )
                        "
                        href="#"
                        class="primary-btn pd-cart"><i class="icon_bag_alt"></i></a>
                  </li>
                  <li class="quick-view">
                    <router-link v-bind:to="'/product/'+itemProduct.id">+ Quick View</router-link>
                  </li>
                </ul>
              </div>
              <div class="pi-text">
                <div class="catagory-name">{{ itemProduct.type }}</div>
                <router-link to="/product">
                  <a href="#">
                    <h5>Mickey Baggy</h5>
                  </a></router-link
                >
                <div class="product-price">
                  {{ itemProduct.price }}
                  <span>$35.00</span>
                </div>
              </div>
            </div>
          </carousel>
        </div>

        <div class="col-lg-12" v-else>
          <p>Produk terbaru belum tersedia</p>
        </div>
      </div>
    </div>
  </section>
  <!-- Women Banner Section End -->
</template>

<script>
import carousel from "vue-owl-carousel";
import axios from "axios";

export default {
  name: "BannerShayna",
  components: {
    carousel,
  },

  data() {
    return {
      products: [],
      keranjangUser: [],
    };
  },
methods: {
    changeImage(urlImage) {
      this.gambar_default = urlImage;
    },

    setDataPicture(data) {
      // replace object productDetails dengan data dari API
      this.productDetails = data;
      // replace value gambar default dengan data dari API (galleries)
      this.gambar_default = data.galleries[0].photo;
    },

    saveKeranjang(idProduct, nameProduct, priceProduct, photoProduct) {
      var productStored = {
        id: idProduct,
        name: nameProduct,
        price: priceProduct,
        photo: photoProduct,
      };

      this.keranjangUser.push(productStored);
      const parsed = JSON.stringify(this.keranjangUser);
      localStorage.setItem("keranjangUser", parsed);

      window.location.reload();
    },
  },
  mounted() {
        if (localStorage.getItem("keranjangUser")) {
      try {
        this.keranjangUser = JSON.parse(localStorage.getItem("keranjangUser"));
      } catch (e) {
        localStorage.removeItem("keranjangUser");
      }
    }
    axios
      .get("http://127.0.0.1:8000/api/products")
      .then((res) => (this.products = res.data.data.data))

      .catch((err) => console.log(err));
  },
};
</script>

<style scoped>
.img-default{
  
  height: 260px;
}
</style>