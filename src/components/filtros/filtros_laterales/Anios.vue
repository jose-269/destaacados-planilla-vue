<template>
  <div class="container px-0 mb-3">
    <div class="row">
      <div class="col">
        <p class="bg-dark text-white fw-bold p-1 text-center rounded-top m-0">
          RANGO DE AÑOS
        </p>
        <div class="container pb-2 shadow p-3 bg-body rounded-bottom">
          <div class="py-4 px-3">
            <vue-slider v-model="observerAnios" 
            :min="Math.min.apply(Math, this.years)" 
            :max="Math.max.apply(Math, this.years)" 
            :lazy="true"
            :interval="1" 
            :process-style="{ backgroundColor: '#DC3545' }"
            :marks="marks" 
            :tooltip="'always'"
            :enable-cross="false">
            <template v-slot:dot="">
              <div :class="['custom-dot']"></div>
            </template>
            </vue-slider>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import VueSlider from "vue-slider-component"
import "vue-slider-component/theme/default.css"
import { mapState, mapMutations } from "vuex";
// import $ from "jquery";
export default {
  name: "Anios",
  data() {
    return {
      anios: [Math.min.apply(Math, this.years), Math.max.apply(Math, this.years)],
    };
  },
  computed: {
    observerAnios: {
      get() {
        return this.minMaxYear;
      },
      set(v) {
         this.setYears(v);
      }
    },
  marks() {
    let arr = [];
    const min = Math.min.apply(Math, this.years);
    const max = Math.max.apply(Math, this.years);
    arr.push(min);
    arr.push(max);
    return arr;
  },    
    ...mapState(["minMaxYear"]),
  },
  methods: {
    ...mapMutations(["getMinMaxYear", "setYears"]),
  },
  components: {
    VueSlider,
  },
  props: {
    years: {
      type: Array,
    },
  },
  created () {
    this.getMinMaxYear();
  },
};
</script>

<style lang="scss" >
.custom-dot {
  width: 150%;
  height: 150%;
  background-color: #fff;
  border: 4px solid #DC3545;
  border-radius: 100%;
  margin-top: -4px;
}
.vue-slider-dot-tooltip-inner   {
  background-color: #DC3545;
  color: white;
}
.vue-slider-dot-tooltip-inner {
  border-top-color: #DC3545;
}
</style>
