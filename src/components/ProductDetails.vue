<template>
<div>
    <section v-if="error">
      {{error.message}}
    </section>
    <section v-else>
      <div v-if="loading">
          <h2 class="loading">Loading...</h2>
      </div>
      <div v-else>
        <h2>{{product.name}}</h2>
        <img :src="product.imageUrl ? product.imageUrl : 'https://placeimg.com/200/200/tech'" width="200" style="float:right" />
        <h3>{{product.description}}</h3>
        <p>Price: {{product.price}}</p>
        <p>Fixed price? {{product.fixedPrice}}</p>
        <p>Discontinued? {{product.discontinued}}</p>
        <p>Modified date: {{product.modifiedDate }}</p>
        <button @click="delProduct">Delete</button>
      </div>
    </section>
  </div>
</template>

<script>
import { mapState, mapActions } from 'vuex'

    export default {
        data () {
            return {
                error: null,
                loading: false
            }
        },
        props: {
            id: {
                type:Number,
                required:true
            }
        },
        methods: {
            delProduct() {
              if (window.confirm("Are you sure ??")) {
                this.deleteProduct(this.product)
                .then(() => {
                  console.info('The product has been deleted.');
                  this.$router.push({ name: 'products'});
                  })
                .catch(error => {
                  console.error('There was an error:', error.response);
                });
              }
            },
            ...mapActions(['fetchProduct','deleteProduct']) // map `this.fetchProduct(this.id)` to `this.$store.dispatch('fetchProduct', this.id)`
        },
        created() {
            this.fetchProduct(this.id);
        },
        computed: {
            ...mapState(['product']), // map `this.product` to `this.$store.state.product`
        },
    }
</script>

<style lang="css" scoped>

</style>