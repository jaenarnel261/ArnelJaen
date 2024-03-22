<template>
    <div>
      <!-- Item Listings -->
      <div>
        <span>Log in: </span>
        <button @click="handleClick">Login</button>
        <h2>Items</h2>
        <div v-for="item in products" :key="item.id" class="item-container">
          <div class="item-info">
            <p>{{ item.name }} - ₱{{ item.price }}</p>
          </div>
          <div class="add-to-cart-btn">
            <button @click="addToCart(item)">Add to Cart</button>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        products: [
          { id: 1, name: 'Item 1', price: 5 },
          { id: 2, name: 'Item 2', price: 10 },
          { id: 3, name: 'Item 3', price: 25 }
        ],
        cart: [],
        isLoggedIn: false // Assuming initially the user is not logged in
      };
    },
    computed: {
      totalPrice() {
        return this.cart.reduce((total, item) => {
          return total + item.product.price * item.quantity;
        }, 0);
      }
    },
    methods: {

        

      addToCart(item)
      {
        if (this.isLoggedIn)
        {
          const foundIndex = this.cart.findIndex(
            (cartItem) => cartItem.product.id === item.id
          );
          if (foundIndex !== -1)
          {
            this.cart[foundIndex].quantity++;
          }
          else
          {
            this.cart.push({ product: item, quantity: 1 });
          }
        }
        else
        {
          const confirmed = window.confirm('Please log in to add items to your cart. Would you like to log in now?');
          if (confirmed)
          {
            localStorage.setItem('token', '12345');
            this.isLoggedIn = true;
            this.$router.push({ name: 'shop' });
            setTimeout(() =>
            {
              window.confirm('You are now logged in. You can add items to your cart.');
            }, 100);
          }
        }
      },
      handleClick()
      {
        localStorage.setItem('token', '12345')
        this.$router.push({ name: 'shop' })
      }
    }
  };
  </script>
  
