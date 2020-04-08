<template>
  <div class="product">
    <HeaderLusy />
    <!-- Breadcrumb Section Begin -->
    <div class="breacrumb-section">
      <div class="container">
        <div class="row">
          <div class="col-lg-12">
            <div class="breadcrumb-text product-more">
              <router-link to="/">
                <i class="fa fa-home"></i> Home
              </router-link>
              <span>Detail</span>
            </div>
          </div>
        </div>
      </div>
    </div>
    <!-- Breadcrumb Section Begin -->

    <!-- Product Shop Section Begin -->
    <section class="product-shop spad page-details">
      <div class="container">
        <div class="row">
          <div class="col-lg-12">
            <div class="row">
              <div class="col-lg-6">
                <div class="product-pic-zoom">
                  <img class="product-big-img" :src="gambar_default" alt />
                </div>
                <div class="product-thumbs" v-if="productDetail.galleries.length > 0">
                  <carousel class="product-thumbs-track ps-slider" :dots="false" :nav="false">
                    <div
                      class="pt"
                      @click="changeImage(thumbs[0])"
                      :class="thumbs[0] === gambar_default ? 'active' : ''"
                    >
                      <img src="img/mickey1.jpg" alt />
                    </div>

                    <div
                      v-for="ss in productDetail.galleries"
                      :key="ss.id"
                      class="pt"
                      @click="changeImage(ss.photo)"
                      :class="ss.photo === gambar_default ? 'active' : ''"
                    >
                      <img v-bind:src="ss.photo" alt />
                    </div>
                  </carousel>
                </div>
              </div>
              <div class="col-lg-6">
                <div class="product-details">
                  <div class="pd-title">
                    <span>{{ productDetail.type }}</span>
                    <h3>{{ productDetail.name }}</h3>
                  </div>
                  <div class="pd-desc">
                    <div v-html="productDetail.description"></div>
                    <h4>${{ productDetail.price }}</h4>
                  </div>
                  <div class="quantity">
                    <router-link to="/cart">
                      <a
                        @click="saveKeranjang(productDetail.id, productDetail.name, productDetail.price, productDetail.galleries[0].photo)"
                        href="#"
                        class="primary-btn pd-cart"
                      >Add To Cart</a>
                    </router-link>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
    <!-- Product Shop Section End -->
    <RelatedProduct />
    <FooterLusy />
  </div>
</template>

<script>
// @ is an alias to /src
import HeaderLusy from "@/components/HeaderLusy.vue";
import FooterLusy from "@/components/FooterLusy.vue";
import RelatedProduct from "@/components/RelatedProduct.vue";
import axios from "axios";
import carousel from "vue-owl-carousel";

export default {
  name: "Product",
  components: {
    HeaderLusy,
    FooterLusy,
    RelatedProduct,
    carousel
  },
  data() {
    return {
      gambar_default: "",
      thumbs: [],
      productDetail: {},
      keranjangUser: []
    };
  },
  methods: {
    changeImage(urlImage) {
      this.gambar_default = urlImage;
    },
    setDataPicture(data) {
      this.productDetail = data;
      this.gambar_default = data.galleries[0].photo;
    },
    saveKeranjang(idProduct, nameProduct, priceProduct, photoProduct) {
      const productStored = {
        id: idProduct,
        name: nameProduct,
        price: priceProduct,
        photo: photoProduct
      };
      this.keranjangUser.push(productStored);
      const parsed = JSON.stringify(this.keranjangUser);
      localStorage.setItem("keranjangUser", parsed);
    }
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
      .get("http://localhost:8000/api/products", {
        params: {
          id: this.$route.params.id
        }
      })
      .then(({ data }) => this.setDataPicture(data.data))
      .catch(err => console.log(err));
  }
};
</script>

<style scoped>
.product-thumbs .pt {
  margin-right: 10px;
}
</style>