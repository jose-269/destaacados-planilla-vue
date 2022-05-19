<template>
  <div class="container-lg">
    <div class="row">
      <div v-if="getCar" class="text-end pe-3 previous-button">
          <router-link to="/seminuevos">
            <button class="btn btn-danger rounded-circle ">
              <i class="fas fa-angle-left fa-2x"></i>
            </button>
          </router-link>
        </div>
      <div class="col-12 col-md-8 mt-4 px-xl-5">
        <!-- CAROUSEL SLIDER -->
        <div class="main-carousel-wrapper" >
          <VueSlickCarousel style="cursor: pointer" ref="c1" :asNavFor="c2" :autoplaySpeed="3000" :arrows="true" :autoplay="true" :speed="500" :focusOnSelect="true" class="images-wrapper">
            <div v-for="img in pictures" :key="img.img" @click="index = img.img" >
              <img :src="img.img" alt="main-pic" class="main-carousel-wrapper__img image" @click="onModal(true)">
            </div>
            <template #prevArrow="arrowOption">
               <div class="custom-arrow-prev">
                {{ arrowOption.currentSlide }}/{{ arrowOption.slideCount }}
              </div>
            </template>
            <template #nextArrow="arrowOption">
               <div class="custom-arrow-next">
                {{ arrowOption.currentSlide }}/{{ arrowOption.slideCount }}
              </div>
            </template>
          </VueSlickCarousel>
        </div>
        <div class="bg-dark indicator-wrapper">
          <div class="indicator-wrapper__within">
            <VueSlickCarousel ref="c2"  :asNavFor="c1" :autoplay="true" :speed="500" :autoplaySpeed="3000" :slidesToShow="6" :arrows="false" :focusOnSelect="true">
              <div v-for="img,i in pictures" :key="i" >
                <img :src="img.img" alt="indicator-pic" class="indicator-carousel" >
              </div>
            </VueSlickCarousel>
          </div>
        </div>
          <!-- FIN CAROUSEL -->
      </div>
      <!-- MODAL -->
      <Lightbox :lightboxPics="pictures" :brandName="getCar.marca" :anio="getCar.anio" :price="getCar.formatPrecio" v-if="showModal" />

      <div class="col-12 col-md-4 mt-4">
        
        <h4 class="text-capitalize">{{ getCar.marca }}</h4>
        <div class="precios">
          <h5>
            <i class="fas fa-star text-warning"></i>Precio: 
            <span class="text-danger fw-bold">{{ getCar.formatPrecio }}</span>
          </h5>
          <h5 class="ms-4">Precio con financiamiento: <span class="text-danger fw-bold">{{ getCar.financingPriceFormat }}</span></h5>
        </div>
        <hr />
        <div class="text-center">
          <img src="../assets/img/banner-2.png" class="img-fluid" alt="banner-1">
        </div>
        <hr />
        <div class="container-fluid px-1">
        <div class="row">
          <div class="col-6">
            <i class="fas fa-circle text-danger me-1"></i>Año
          </div>
          <div class="col-6">
            <p class="mb-0 d-inline-block">{{ getCar.anio }}</p>
          </div>
          <div class="col-6 ">
            <i class="fas fa-circle text-danger me-1"></i>Kilometraje
          </div>
          <div class="col-6 ">{{ getCar.formatKms }}</div>
          <div class="col-6">
            <i class="fas fa-circle text-danger me-1"></i>Transmisión
          </div>
          <div class="col-6 text-capitalize">{{ getCar.transmision }}</div>
          <div class="col-6 ">
            <i class="fas fa-circle text-danger me-1"></i>Combustible
          </div>
          <div class="col-6 text-capitalize">{{ getCar.combustible }}</div>
          <div class="col-6">
            <i class="fas fa-circle text-danger me-1"></i>Cilindrada
          </div>
          <div class="col-6">{{ getCar.cilindrada }}</div>
          <div class="col-6">
            <i class="fas fa-circle text-danger me-1"></i>Color
          </div>
          <div class="col-6 text-capitalize">{{ getCar.color }}</div>
        </div>
        </div>
        <div class="cotiza border border-warning mt-3">
          <div class="form-group py-4 px-3">
            <h4 class="text-center">
              <i class="fas fa-star text-warning me-1"></i>COTIZAR ESTE VEHÍCULO
            </h4>
            <PageJuntos v-if="pageNombres === true" />
            <PageSeparados v-else />
            <div class="row mb-3">
              <div class="col-6">
                <input
                  type="phono"
                  class="form-control"
                  placeholder="Teléfono"
                />
              </div>
              <div class="col-6">
                <input type="text" class="form-control" placeholder="E-mail" />
              </div>
            </div>
            <textarea
              class="form-control mb-4"
              name="mensaje"
              cols="30"
              rows="2"
              placeholder="Mensaje"
            ></textarea>
            <div class="form-check pb-2">
              <input
                class="form-check-input"
                type="checkbox"
                data-bs-toggle="collapse"
                data-bs-target="#collapse-credito"
              />
              <label class="form-check-label label-credito">
                COTIZA TU CRÉDITO
              </label>
            </div>
            <div id="collapse-credito" class="collapse">
              <input
                type="text"
                class="form-control form-control-sm mb-2"
                placeholder="Pie"
              />
              <h6 class="label-credito ms-2">Cuotas</h6>
              <div class="form-check form-check-inline">
                <input class="form-check-input" type="checkbox" />
                <label class="form-check-label label-credito">12</label>
              </div>
              <div class="form-check form-check-inline">
                <input class="form-check-input" type="checkbox" />
                <label class="form-check-label label-credito">24</label>
              </div>
              <div class="form-check form-check-inline">
                <input class="form-check-input" type="checkbox" />
                <label class="form-check-label label-credito">36</label>
              </div>
              <div class="form-check form-check-inline">
                <input class="form-check-input" type="checkbox" />
                <label class="form-check-label label-credito">48</label>
              </div>
              <div class="form-check form-check-inline pb-2">
                <input class="form-check-input" type="checkbox" />
                <label class="form-check-label label-credito">60</label>
              </div>
            </div>

            <div class="form-check pb-2">
              <input
                class="form-check-input"
                type="checkbox"
                data-bs-toggle="collapse"
                data-bs-target="#parte-de-pago"
              />
              <label class="form-check-label label-credito">
                DEJA TU AUTO EN PARTE DE PAGO
              </label>
            </div>
            <div id="parte-de-pago" class="collapse">
              <input
                type="text"
                class="form-control form-control-sm mb-2"
                placeholder="Automóvil, marca, modelo, versión, año, kilometraje, etc"
              />
            </div>
          </div>
        </div>
        <div class="text-end">
          <button class="btn btn-danger px-4 enviar-cotizacion me-3">
            Enviar
          </button>
        </div>
      </div>
    </div>
    <div class="row mb-5">
      <div class="col-12 col-md-8 mt-3">
        <div class="border p-4 shadow h-100">
          <h5><i class="fas fa-star text-warning me-1"></i>Equipamiento</h5>
          <div class="pt-4 px-4">
            <ul class="des-lista p-0">
              <li>
                <i class="fas fa-check text-danger me-2 pb-3"></i>Aire
                Acondicionado
              </li>
              <li><i class="fas fa-check text-danger me-2 pb-3"></i>Alarma</li>
              <li>
                <i class="fas fa-check text-danger me-2 pb-3"></i>Cierre
                centralizado
              </li>
              <li>
                <i class="fas fa-check text-danger me-2 pb-3"></i>Dirección
              </li>
              <li>
                <i class="fas fa-check text-danger me-2 pb-3"></i>4 Puertas
              </li>
              <li>
                <i class="fas fa-check text-danger me-2 pb-3"></i>4 Air bag
              </li>
              <li>
                <i class="fas fa-check text-danger me-2 pb-3"></i>Alzavidrios
                Eléctricos delanteros
              </li>
              <li><i class="fas fa-check text-danger me-2 pb-3"></i>Cuero</li>
              <li>
                <i class="fas fa-check text-danger me-2 pb-3"></i>Frenos ABS
              </li>
            </ul>
          </div>
          <h5><i class="fas fa-star text-warning me-1"></i>Descripción</h5>
        </div>
      </div>
      <div class="col-12 col-md-4 mt-3">
        <div class="border p-4 shadow">
          <div class="d-flex justify-content-between">
            <div>
              <h5><i class="fas fa-star text-warning me-1"></i>Ubicación</h5>
              <a href="#" style="text-decoration: none">+56-51-2296917</a>
            </div>
            <div class="waze-button my-auto">
              <a href="https://ul.waze.com/ul?ll=-33.45408800%2C-70.60303410&navigate=yes&utm_campaign=waze_website&utm_source=waze_website&utm_medium=lm_share_location" target="_blank">
              <i class="fab fa-waze fa-2x p-1"></i></a>
            </div>
          </div>
          <div class="mapa pt-4">
            <iframe
              src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d426000.965030887!2d-70.91001965610009!3d-33.47189991731077!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x9662c5410425af2f%3A0x8475d53c400f0931!2sSantiago%2C%20Regi%C3%B3n%20Metropolitana!5e0!3m2!1ses!2scl!4v1588798327102!5m2!1ses!2scl"
              width="100%"
              height="200"
              frameborder="0"
              style="border: 0; margin-bottom: -7px"
              allowfullscreen=""
              aria-hidden="false"
              tabindex="0"
            ></iframe>
          </div>
        </div>
      </div>
    </div>
    <div class="row">
      <div class=" text-center">
          <img src="../assets/img/banner-bajo-el-precio.png" class="img-fluid" alt="banner-1">
        </div>
    </div>
    <div class="row mb-5">
      <div class="col-12">
        <h2 class="pb-4">Sugeridos</h2>
        <div class="card mb-5" style="width: 18rem">
          <div class="card-header bg-warning">
            <h4 class="mb-0 text-white">Diesel</h4>
          </div>
          <img src="../assets/img/auto-foto.png" class="pb-3" alt="" />
          <div class="container py-3">
            <h5 class="card-title pb-2">
              <i class="fas fa-star text-warning"></i>AUDI Q3
            </h5>
            <h5 class="card-title pb-2">$ 2199000</h5>
            <div class="row justify-content-around">
              <div class="col fw-ligth">2018</div>
              <div class="col fw-ligth">Mecánica</div>
              <div class="col fw-ligth">98000 Kms</div>
            </div>
          </div>
          <button class="btn btn-danger">VER MÁS</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { mapMutations, mapState } from "vuex";
import VueSlickCarousel from 'vue-slick-carousel';
import 'vue-slick-carousel/dist/vue-slick-carousel.css';
import 'vue-slick-carousel/dist/vue-slick-carousel-theme.css';
import Lightbox from '@/components/Lightbox';
import PageJuntos from "@/components/filtros/filtro-nombres-page/PageNombresJuntos";
import PageSeparados from "@/components/filtros/filtro-nombres-page/PageNombresSeparados";
// import Slider from "@/components/ficha-detalle/Slider.vue";
export default {
  data() {
    return {
      
      pictures: [
        {
          img: require("@/assets/img/carousel/foto-ZZZZ55-1.jpg")
        },
         {
          img: require("@/assets/img/carousel/foto-ZZZZ55-2.jpg")
        },
         {
          img: require("@/assets/img/carousel/foto-ZZZZ55-3.jpg")
        },
         {
          img: require("@/assets/img/carousel/foto-ZZZZ55-4.jpg")
        },
         {
          img: require("@/assets/img/carousel/foto-ZZZZ55-5.jpg")
        },
         {
          img: require("@/assets/img/carousel/foto-ZZZZ55-6.jpg")
        },
         {
          img: require("@/assets/img/carousel/foto-ZZZZ55-7.jpg")
        },
         {
          img: require("@/assets/img/carousel/foto-ZZZZ55-8.jpg")
        },
         {
          img: require("@/assets/img/carousel/foto-ZZZZ55-9.jpg")
        },
      ],
      settingsIndicators: {
        "dots": false,
        "arrows": true,
        "edgeFriction": 0.35,
        "infinite": true,
        "speed": 500,
        // "slidesToShow": 9,
        "slidesToScroll": 1,
        "autoplay": true,
        // "focusOnSelect": true,
        "asNavFor": "refs.main"
      },
      index: null,
      active: 0,
      c1: undefined,
      c2: undefined,
    };
  },
  methods: {
    ...mapMutations(['setModal']),
    setActive(index) {
      let active = index;
      if (index === this.pics.length) active;
      else if (index === -1) active = this.pics.length - 1;
      this.active = active;
    },
    onModal(on) {
      this.setModal(on);
    }
  },
  computed: {
    ...mapState(["pageNombres", "data", "showModal"]),
    id() {
      return this.$route.params.id;
    },
    getCar() {
      let data = this.data;
      const item = data.find(car => car.id === this.$route.params.id);
      return item;
    }
  },
  components: {
    PageJuntos,
    PageSeparados,
    VueSlickCarousel,
    Lightbox,
  },
  mounted () {
    this.c1 = this.$refs.c1;
    this.c2 = this.$refs.c2;
  },
  watch: {
    showModal: function() {
      if(this.showModal){
        document.documentElement.style.overflow = 'hidden'
        return
      }
      document.documentElement.style.overflow = 'auto'
    }
  },
};
</script>

<style lang="scss" scoped>

  .previous-button {
    margin-top: -23px;
    position: relative;
    right: 0px;
    z-index: 1;
  }
  .enviar-cotizacion {
    margin-top: -20px;
  }
  .des-lista {
    columns: 2;
    list-style: none;
    margin-right: 0;
  }
  .textDes {
    padding: 1% 2%;
    color: #000;
    font-size: 0.6875rem;
    text-align: justify;
  }
  .label-credito {
    font-size: 13px;
  }
  .waze-button {
    background-color: #fff;
    height: fit-content;
    border: 4px solid #42D5FF;
    border-radius: 13px 13px;
    a {
      color: #395359;
    }
  }
  .main-carousel-wrapper {
    width: 90%;
    margin: 0 auto;

    &__img {
      width: 100%;
    }
  }
  .indicator-carousel {
    width: 100%;
    padding: 0 0.3125rem;
    cursor: pointer;
    // margin: 0 -50px;
    // border:  -5px solid black;
  }
.indicator-wrapper {
  overflow: hidden;
  width: 90%; 
  margin:-7px auto;
  border-top: 7px solid #212529;

  &__within {
    margin: 0 -4.8px 0 -4.8px;
  }
}
div.slick-slide {
  width: 500px;
}
.custom-arrow-prev {
  z-index: 200;
}
.custom-arrow-next {
  z-index: 200;
}

.slick-prev::before {
  color: rgb(255, 255, 255) !important;
  font-size: 50px;
  line-height: 1;
  position: absolute;
  display: inline-block;
  left: 31px;
  opacity: 0.75;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

.slick-next {
  right: 633px;
}

.slick-next::before {
  color: rgb(255, 255, 255) !important;
  font-size: 50px;
  line-height: 1;
  position: absolute;
  display: inline-block;
  left: 596px;
  opacity: 0.75;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}


@media (max-width: 767px) {
  .main-carousel-wrapper {
    width: 100%;
  }
  .indicator-wrapper {
    width: 100%; 
    margin:-7px auto;
    border-top: 7px solid #212529;
  }
}
@media (max-width: 567px) {
  .indicator-carousel {
    width: 100%;
    padding: 0 2px;

  }
}

</style>
