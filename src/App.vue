<style>
  @import './assets/css/styles.css';
</style>

<template>

  <div id="app">
    <header id="navbar">
      <h1 id="sitetitle" v-text="sitename"></h1>
      <button v-show="btnCart" id="btnCart" v-on:click='showCart'>
        <!--Returns the Number of Items Inside the Cart.-->
        {{cartItemCount}}
        <span id="btnText" class="fas fa-cart-plus" v-on:click='switchText'> Cart</span>
      </button>
    </header>
    <main>
      <div id="box" v-if='showLessonsPage'>
          <LessonList @addLesson='addToCart'/>
      </div>
      <!--If the show lessons page == false then it will switch to the cart section and display the cart section on the app -->
      <div id="cart-page" v-else>
        <!--Switching to the Cart Page-->
        <!--Displaying The Checkout (add the component below here)-->
          <CartAndCheckout :cart="cart" @removeFromCart='removeLesson'></CartAndCheckout>
      </div>
    </main>
  </div>
</template>

<script>
import LessonList from './components/LessonList.vue'
import CartAndCheckout from './components/CartAndCheckout.vue'

export default {
  name: 'App',
  components: {
    LessonList,
    CartAndCheckout
  },
  data () {
    return {
      sitename: 'Lessons-App',
      cart: [],
      showLessonsPage: true,
      btnCart: true,
      sort: '',
      checkout: {
        Name: '',
        PhoneNo: '',
      },
      checkoutMsg: false, // checkout message which is hidden at the moment. 
      orders: [],
    }
  },
  mounted() { //This is used when the page first loads up, this will execute everyting in the beggining. 
    this.btnCart = false;   //Button is hidden as no items in the cart have been added.
  },
  methods: {
    addToCart(lesson) {
      this.btnCart = true;
      this.cart.push(lesson)
      JSON.stringify({ "Space": this.counter(lesson) })
      console.log(this.cart)
    },
    counter(lesson) {
                    let count = 5;
                    //This for-loop is going to go through everyting inside of the cart. 
                    for (let i = 0; i < this.cart.length; i++) {
                        //If an element of the cart is equal/equal type to the input(lesson) then, the count will increase by 1.
                        if (this.cart[i] === lesson) {
                            count--;
                        }
                    }
                    return count; //Returns the count of how many have been identified.
                },
    removeLesson(cart) {
      const index = this.cart.indexOf(cart);    //Finds the index of the Cart Array. 
      if (index !== -1) { //If index of the cart array is not equal value or equal type. 
        this.cart.splice(index, 1);   //Splice is used to remove that index. 
      } 

      //This function will execute when cart == 0.
      if (this.cart == 0) {
        location.reload();
        this.btnCart = false; //As the cart has 0 lessons the button will not be visible.
        this.showLessonsPage = this.showLessonsPage ? false : true; //This will Automatticly chnage to the lesson webpage from the cart page.
      }
      
    },
    showCart() {
      //Getting the Cart for displaying.
      this.showLessonsPage = this.showLessonsPage ? false : true; //Turn the message to false to display the cart. 
    },
    switchText() {  //This is used to change the button text as you switch beetween the lessons and cart page.
      let btnText = document.getElementById("btnText");
        if (btnText.innerHTML === " Cart") {
            btnText.innerHTML = " Lessons";
        } else {
            btnText.innerHTML = " Cart";
        }
    },
    cartCount(lesson) { //Finding a lesson element from the cart which will equal the same lesson. 
      let count = 0;
      //This for-loop is going to go through everyting inside of the cart. 
      for (let i = 0; i < this.cart.length; i++) {
        //If an element of the cart is equal/equal type to the input(lesson) then, the count will increase by 1.
        if (this.cart[i] === lesson) {
          count++;
        }
      }
      return count; //Returns the count of how many have been identified.
    },
    canAddToCart(lesson) {
      //Comparing how many is already in the cart with the actual lesson space. 
      return lesson.Space > this.cartCount(lesson);
    },
  },
  computed: {
    cartItemCount() {
      return this.cart.length; //Returns the number of items inside the cart and will update on the increse and decrease.
    },
  }
}
</script>
