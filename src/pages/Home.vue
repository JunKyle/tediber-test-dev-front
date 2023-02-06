<template>
  <main class="Home">
    <section class="Home__section">
      <div class="Home__view">
        <img class="Home__viewImage"
             src="/images/slider.jpeg" />
      </div>
      <div class="Home__content"
           v-if="product">
        <h1 class="Home__title">{{ product.name }}</h1>
        <ul class="Home__description">
          <li class="Home__descriptionItem">Canapé 2-3 places design et ultra confortable</li>
          <li class="Home__descriptionItem">Matériaux et design haut de gamme à un prix incomparable</li>
          <li class="Home__descriptionItem">Fabriqué en France et éco-conçu</li>
          <li class="Home__descriptionItem">Entretien facile, coussins déhoussables et lavables</li>
        </ul>
        <h2 class="Home__subtitle">Je paramètre mon canapé</h2>
        <form class="Form"
              @submit="addToCart">
          <template v-for="(option, index) in product.options">
            <div class="Form__section"
                 :key="index">
              <div class="Form__title">
                <span class="Form__titleNumber">{{index + 1}}</span>
                <h3 class="Form__titleLabel">{{option.name}}</h3>
              </div>
              <div class="Form__input"
                   v-if="option.code.includes(code + '-size')">
                <select v-model="formOptions[code + '-size']">
                  <option v-for="(optionValue, index) in option.values"
                          :value="optionValue.code"
                          :key="index">
                    {{ optionValue.value }}
                  </option>
                </select>
              </div>
              <template v-if="option.code.includes(code + '-color')">
                <div class="Form__input Form__input--grid3">
                  <div class="RadioButton"
                       v-for="(optionValue, index) in option.values"
                       :key="index">
                    <input class="RadioButton__input"
                           type="radio" 
                           :id="optionValue.code" 
                           :value="optionValue.code" 
                           v-model="formOptions[code + '-color']">
                    <span class="RadioButton__color"
                          :class="'RadioButton__color--' + optionValue.value.replace(' ', '')"></span>
                    <label class="RadioButton__label"
                           :for="optionValue.code">{{ optionValue.value }}</label>
                  </div>
                </div>
              </template>
              <template v-if="option.code.includes(code + '-feet-form')">
                <span class="Form__info"><i class="fa fa-info-circle"></i>Les pieds mesurent 12 cm de hauteur</span>
                <div class="Form__input Form__input--grid2">
                  <div class="RadioButton RadioButton--square"
                       v-for="(optionValue, index) in option.values"
                       :key="index">
                    <input class="RadioButton__input"
                           type="radio" 
                           :id="optionValue.code" 
                           :value="optionValue.code" 
                           v-model="formOptions[code + '-feet-form']">
                    <img class="RadioButton__image"
                         :src="getImageOption(optionValue)" />
                    <label class="RadioButton__label"
                           :for="optionValue.code">{{ optionValue.value }}</label>
                  </div>
                </div>
              </template>
              <template v-if="option.code.includes(code + '-feet-color')">
                <div class="Form__input Form__input--grid3">
                  <div class="RadioButton"
                       v-for="(optionValue, index) in option.values"
                       :key="index">
                    <input class="RadioButton__input"
                           type="radio" 
                           :id="optionValue.code" 
                           :value="optionValue.code" 
                           v-model="formOptions[code + '-feet-color']">
                    <img class="RadioButton__image"
                         :src="getImageOption(optionValue)" />
                    <label class="RadioButton__label"
                           :for="optionValue.code">{{ optionValue.value }}</label>
                  </div>
                </div>
              </template>
            </div>
          </template>
          <span class="Form__price">{{productVariantPrice}}</span>
          <input class="Form__submit" 
                 type="submit"
                 value="Ajouter au panier">
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
      productVariant: null,
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
  computed: {
    productVariantPrice () {
      return this.productVariant && this.productVariant.price ? this.productVariant.price + ' €' : '';
    }
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
        window.alert('Ajout au panier : ' + this.productVariant.name +' => { code : ' + this.productVariant.code + '}')
        this.$http.post('./data/api.json', { code : this.productVariant.code})
            .then(response => console.log('panier maj', response));
      }
    },
    getImageOption (option) {
      return '/images/' + option.code + (option.code.includes(this.code + '-feet-form') ? '.jpeg' : '.png');
    }
  }
}
</script>

<style lang="scss" scoped>
.Home {
  margin: 20px 0;

  &__section {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: flex-start;
    height: 100vh;
    overflow-x: hidden;
    overflow-y: scroll;

    &::-webkit-scrollbar {
      display: none;
    }

    -ms-overflow-style: none;  /* IE and Edge */
    scrollbar-width: none;  /* Firefox */
  }

  &__view {
    flex: 2 1 0;    
    position: sticky;
    top: 0;
    max-height: 80vh;

    &Image {
      width: 100%;
    }
  }

  &__content {
    flex: 1 1 0;
    margin: 0 10px;
    padding-bottom: 300px;
  }

  &__title {
    margin: 0;
    color: $color-dark-blue;
    font-size: 26px;
  }

  &__description {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    grid-column-gap: 15px;
    grid-row-gap: 15px;
    background-color: $color-grey1;
    color: $color-dark-blue;
    font-size: 10px;
    padding: 15px 25px;

    &Item {
      text-align: left;
    }
  }

  &__subtitle {
    border-top: 1px solid $color-black;
    padding: 20px 0;
    color: $color-dark-blue;
    font-size: 18px;
  }
}
</style>