<template>
<div class="vue-world-map">
  <Worldmap />
</div>
</template>

<script>
import chroma from 'chroma-js';
import Worldmap from './Map.vue';
import {
  getDynamicMapCss,
  getBaseCss,
  getCombinedCssString,
} from './dynamic-map-css';


export default {
  components: { Worldmap },
  watch: {
      countryData: {
          handler(val,old){
              console.log('change')
              this.renderMapCSS();
          },
          deep: true
      }
      /*
    countryData: {
        handler:()=>{
            console.log('change')
            this.renderMapCSS();
        },deep:true

    },
    */
  },
  props: {
    lowColor: {
      type: String,
      default: '#fde2e2',
    },
    highColor: {
      type: String,
      default: '#d83737',
    },
    countryData: {
      type: Object,
      required: true,
    },
    defaultCountryFillColor: {
      type: String,
      default: '#dadada',
    },
    countryStrokeColor: {
      type: String,
      default: 'black',
    },
  },
  data() {
    return {
      node: document.createElement('style'),
      chromaScale: chroma.scale([this.$props.lowColor, this.$props.highColor]),
    };
  },
  methods: {
    renderMapCSS() {
      const baseCss = getBaseCss(this.$props);
      const dynamicMapCss = getDynamicMapCss(this.$props.countryData, this.chromaScale);
      this.$data.node.innerHTML = getCombinedCssString(baseCss, dynamicMapCss);
      this.$data.node.id = 'vueWorldMap'
    },
  },
  mounted() {
      this.renderMapCSS();
      if(document.getElementById('vueWorldMap')){
          document.getElementById('vueWorldMap').innerHTML=this.$data.node.innerHTML;
      }else{
          document.body.appendChild(this.$data.node);
      }


  },
};
</script>

<style>
.vue-world-map {
  height: 100%;
}

#map-svg {
  height: 100%;
}
</style>
