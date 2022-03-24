<style>
  @import '../assets/css/styles.css';
</style>

<template>
    <div>
        <center>
            <h1>Cart</h1>
        </center>
        <div id="display-cart" v-for='(lesson, index) in cart' :key="index">
            <!--cart is one elemnt of the displayLessonsCart Array-->
            <h1>{{lesson.Subject}}</h1>
            <p>Location: {{lesson.Location}}</p>
            <h3>Price: £{{lesson.Price}}</h3>
            <h4>Spaces: {{lesson.Space}}</h4>
            <img v-bind:src="lesson.Image" height="60px" width="60px"><br><br>
            <!--This button removes the lesson from the cart, cart is know as one lesson inside the cart array-->
            <button @click="remove(lesson)">Remove</button>
        </div>
        <div id="checkout-section">
            <!--Displaying Cart Props-->
            <br>
            <h2>Checkout</h2>

            <p>
                <strong>Name:</strong>
                <!-- This input field is for entering the Name. -->
                <input v-model.trim="name" />
            </p>
             <p>
                <strong>Phone Number:</strong>
                <!-- This input field is for entering the Phonenumber. -->
                <input v-model.trim="Phonenumber" />
            </p>

            <h2>Order Information</h2>
            <p>First Name: {{name}}</p>
            <p>Phone Number: {{Phonenumber}}</p>
            <!--If the users inputted name and phoneNo is both true it will display the checkout button else it won't-->
            <!--Once it's true then, the user can purchase-->
            <button @click="purchase()" v-if="checkCheckout">Checkout</button>
            <p v-show="checkoutMsg" id="msg">
                Your Order has been purhcased.
                Wait 5 Seconds as you are being redirected...
            </p>
        </div>
    </div>

</template>

<script>
export default {
    name: "CartAndCheckout",
    props: ['cart'],
    data() {
        return {
            name: "",
            Phonenumber: "",
            checkoutMsg: false,
        };
    },
    methods: {
        remove(lesson) {
            this.$emit("removeFromCart", lesson);
        },
        validateCheckout(name, Phonenumber) {   //This function is used to validate the name and the phonenumber.
            let regExname = /^[A-Za-z]+$/;
            let regExphoneNumber = /^[\s()+-]*([0-9][\s()+-]*){11,20}$/;
            return regExname.test(name) && regExphoneNumber.test(Phonenumber);  //It will check the name and phonenumber with each of their regex.
        },
        purchase() {
            this.checkoutMsg = true;
             setTimeout(function () {
                location.reload();
            }, 5000);   //Refreshes after 5 seconds. 
        }
    },
    computed: {
        checkCheckout() { //This function checks the validation of the name and phonenumber inputted by the validate function.
            if (!this.validateCheckout(this.name, this.Phonenumber)) {
                return false;   //Returns false which means either the name, phonoNo, or Both are incorrect.
            } else {
                return true;    //Returns true if both matches the regex. 
            }
        },
    }
};
</script>