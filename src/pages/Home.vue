<template>
  <main class="Home">
    <section class="Home__section">
      <div class="Home__view">
        <img class="Home__viewImage"
             src="@/assets/static/images/slider.jpeg" />
      </div>
      <div class="Home__content"
           v-if="product">
        <h1 class="Home__title">{{ product.name }}</h1>
        <ul class="Home__description">
          <li>Canapé 2-3 places design et ultra confortable</li>
          <li>Matériaux et design haut de gamme à un prix incomparable</li>
          <li>Fabriqué en France et éco-conçu</li>
          <li>Entretien facile, coussins déhoussables et lavables</li>
        </ul>
        <h2 class="Home__subtitle">Je paramètre mon canapé</h2>
        <form @submit="addToCart">
          <template v-for="(option, index) in product.options">
            <select v-model="formOptions[code + '-size']"
                    :key="index"
                    v-if="option.code.includes(code + '-size')">
              <option v-for="(optionValue, index) in option.values"
                      :value="optionValue.code"
                      :key="index">
                {{ optionValue.value }}
              </option>
            </select>
            <div v-if="option.code.includes(code + '-color')"
                  :key="index">
              <div v-for="(optionValue, index) in option.values"
                   :key="index">
                <input type="radio" 
                       :id="optionValue.code" 
                       :value="optionValue.code" 
                       v-model="formOptions[code + '-color']">
                <label :for="optionValue.code">{{ optionValue.value }}</label>
              </div>
            </div>
            <div v-if="option.code.includes(code + '-feet-form')"
                  :key="index">
              <div v-for="(optionValue, index) in option.values"
                   :key="index">
                <input type="radio" 
                       :id="optionValue.code" 
                       :value="optionValue.code" 
                       v-model="formOptions[code + '-feet-form']">
                <label :for="optionValue.code">{{ optionValue.value }}</label>
              </div>
            </div>
            <div v-if="option.code.includes(code + '-feet-color')"
                  :key="index">
              <div v-for="(optionValue, index) in option.values"
                   :key="index">
                <input type="radio" 
                       :id="optionValue.code" 
                       :value="optionValue.code" 
                       v-model="formOptions[code + '-feet-color']">
                <label :for="optionValue.code">{{ optionValue.value }}</label>
              </div>
            </div>
          </template>
          <input type="submit"
                 value="Submit">
        </form>
      </div>
    </section>
  </main>
</template>

<script>
export default {
  name: 'Home',
  props: {
    code: String
  },
  data: function () {
    return {
      product: null,
      formOptions: null
    }
  },
  watch: {
    formOptions: {
      handler () {
        this.setVariant ();
      },
      deep: true
    }
  },
  mounted () {
    this.getHomeData ();
  },
  methods: {
    getHomeData () {
       this.$http
        .get('./data/api.json?code=' + this.code)
        .then(response => {
          if (response && response.data) {
            this.product = response.data;
            this.initFormOptions ();
            this.setVariant ();
          }
        });
    },
    initFormOptions () {
      console.log('this.code', this.code);
      this.formOptions = {
        [this.code + '-size']: this.product.options.find(option => option.code.includes('size')).defaultOptionValue,
        [this.code + '-color']: this.product.options.find(option => option.code.includes('color')).defaultOptionValue,
        [this.code + '-feet-form']: this.product.options.find(option => option.code.includes('feet-form')).defaultOptionValue,
        [this.code + '-feet-color']: this.product.options.find(option => option.code.includes('feet-color')).defaultOptionValue
      }
    },
    setVariant () {
      if (this.product && this.product.variants) {
        this.productVariant = this.product.variants.find(item => JSON.stringify(item.optionCodes) === JSON.stringify(this.formOptions));
      }
    },
    addToCart () {
      if (this.productVariant && this.productVariant.code) {
        this.$http.post('./data/api.json', { code : this.productVariant.code})
            .then(response => console.log('panier maj', response));
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.Home {

  &__section {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: flex-start;
  }

  &__view {

    &Image {
      width: 100%;
    }
  }

  &__content {
    min-width: 30%;
  }

}
</style>