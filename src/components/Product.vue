<template>
  <div class="product">
    <div class="product-image">
      <img v-bind:src="image"/>
    </div>
    <div class="product-info">
      <h1>{{ title }}</h1>
      <p>Shipping: {{ shipping }}</p>
      <p v-if="inStock">In Stock</p>
      <p v-else>Out of Stock</p>
      <ul>
        <li v-for="detail in details">{{detail}}</li>
      </ul>
      <div
        v-for="(variant,index) in variants" :key="variant.variantId"
        class="color-box" :style="{backgroundColor: variant.variantColor}"
        @mouseover="updateProduct(index)"
      >
      </div>
      <button v-on:click="addToCart" :disabled="!inStock" :class="{disabledButton:!inStock}">Add to Cart</button>
      <button v-on:click="removeFromCart">Remove from Cart</button>

    </div>
  </div>
</template>

<script>
  export default {
    props: {
      premium: {
        type: Boolean,
        required: true,
      },
    },
    data() {
      return {
        product: 'Socks',
        brand: 'Mike',
        selectedVariant: 1,
        variants: [
          {
            variantId: 1,
            variantColor: 'red',
            variantImage: './img/icons/vmSocks-green.jpg',
            variantQuantity: 5,
          },
          {
            variantId: 2,
            variantColor: 'blue',
            variantImage: './../img/icons/vmSocks-blue.jpg',
            variantQuantity: 15,
          },
        ],
        details: ['Summmer', 'WollenS', 'Green-weave'],
        cart: 0,
      };
    },
    methods: {
      addToCart() {
        this.variants[this.selectedVariant].variantQuantity -= 1;
        this.$emit('add-to-cart');
      },
      updateProduct(index) {
        this.selectedVariant = index;
      },
      removeFromCart() {
        this.variants[this.selectedVariant].variantQuantity += 1;
        this.cart -= 1;
      },
    },
    computed: {
      title() {
        return `${this.brand} ${this.product}`;
      },
      image() {
        return this.variants[this.selectedVariant].variantImage;
      },
      inStock() {
        if (this.variants[this.selectedVariant].variantQuantity > 0) {
          return true;
        }
        return false;
      },
      shipping() {
        if (this.premium) {
          return 'Free';
        }
        return 2.99;
      },
    },
  };
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
  h1, h2 {
    font-weight: normal;
  }

  ul {
    list-style-type: none;
    padding: 0;
  }

  li {
    display: inline-block;
    margin: 0 10px;
  }

  a {
    color: #35495E;
  }
  .color-box {
    width: 40px;
    height: 40px;
    margin-top: 5px;
  }
  .product {
    display: flex;
    flex-flow: wrap;
    padding: 1rem;
  }

  img {
    border: 1px solid #d8d8d8;
    width: 70%;
    margin: 40px;
    box-shadow: 0px .5px 1px #d8d8d8;
  }

  .product-image {
    width: 80%;
  }

  .product-image,
  .product-info {
    margin-top: 10px;
    width: 50%;
  }

</style>
