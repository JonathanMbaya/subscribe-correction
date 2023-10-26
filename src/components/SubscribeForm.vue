<template>
  <div class="container">
    <form @submit.prevent="onSubmit">
      <div class="row">
        <h4>Account</h4>
        <div class="error" v-if="v$.fullName.$error">
          {{ v$.fullName.$errors[0].$message }}
        </div>
        <div class="input-group input-group-icon">
          <input type="text" placeholder="Full Name" v-model="fullName" />
          <div class="input-icon"><i class="fa fa-user"></i></div>
        </div>
        <div class="error" v-if="v$.email.$error">
          {{ v$.email.$errors[0].$message }}
        </div>
        <div class="input-group input-group-icon">
          <input type="email" placeholder="Email Adress" v-model="email" />
          <div class="input-icon"><i class="fa fa-envelope"></i></div>
        </div>
        <div class="error" v-if="v$.password.$error">
          {{ v$.password.$errors[0].$message }}
        </div>
        <div class="input-group input-group-icon">
          <input type="password" placeholder="Password" v-model="password" />
          <div class="input-icon"><i class="fa fa-key"></i></div>
        </div>
      </div>
      <div class="row">
        <div class="col-half">
          <h4>Date of Birth</h4>
          <div class="input-group">
            <div class="col-third">
              <input type="text" placeholder="DD" v-model="birth.day" />
            </div>
            <div class="col-third">
              <input type="text" placeholder="MM" v-model="birth.month" />
            </div>
            <div class="col-third">
              <input type="text" placeholder="YYYY" v-model="birth.year" />
            </div>
            <div
              class="error"
              v-if="
                v$.birth.day.$error ||
                v$.birth.month.$error ||
                v$.birth.year.$error
              "
            >
              {{
                [
                  ...v$.birth.day.$errors,
                  ...v$.birth.month.$errors,
                  ...v$.birth.year.$errors,
                ][0].$message
              }}
            </div>
          </div>
        </div>
        <div class="col-half">
          <h4>Gender</h4>
          <div class="input-group">
            <input
              type="radio"
              name="gender"
              value="male"
              id="gender-male"
              v-model="gender"
            />
            <label for="gender-male">Male</label>
            <input
              type="radio"
              name="gender"
              value="female"
              id="gender-female"
              v-model="gender"
            />
            <label for="gender-female">Female</label>
            <div class="error" v-if="v$.gender.$error">
              {{ v$.gender.$errors[0].$message }}
            </div>
          </div>
        </div>
      </div>
      <div class="row">
        <h4>Payment</h4>
        <div class="error" v-if="v$.card.number.$error">
          {{ v$.card.number.$errors[0].$message }}
        </div>
        <div class="input-group input-group-icon">
          <input
            type="text"
            placeholder="Card Number"
            v-model="card.number"
            maxlength="16"
          />
          <div class="input-icon"><i class="fa fa-credit-card"></i></div>
        </div>
        <div class="col-half">
          <div class="input-group input-group-icon">
            <input
              type="text"
              placeholder="Card CVC"
              v-model="card.cvc"
              maxlength="3"
            />
            <div class="input-icon"><i class="fa fa-user"></i></div>
          </div>
          <div class="error" v-if="v$.card.cvc.$error">
            {{ v$.card.cvc.$errors[0].$message }}
          </div>
        </div>
        <div class="col-half">
          <div class="input-group">
            <select v-model="card.expireMonth">
              <option value="1">01</option>
              <option value="2">02</option>
              <option value="3">03</option>
              <option value="4">04</option>
              <option value="5">05</option>
              <option value="6">06</option>
              <option value="7">07</option>
              <option value="8">08</option>
              <option value="9">09</option>
              <option value="10">10</option>
              <option value="11">11</option>
              <option value="12">12</option>
            </select>

            <select v-model="card.expireYear">
              <option value="2020">2020</option>
              <option value="2021">2021</option>
              <option value="2022">2022</option>
              <option value="2023">2023</option>
              <option value="2024">2024</option>
              <option value="2025">2025</option>
              <option value="2026">2026</option>
              <option value="2027">2027</option>
            </select>
          </div>
          <div
            class="error"
            v-if="v$.card.expireMonth.$error || v$.card.expireYear.$error"
          >
            {{
              [...v$.card.expireMonth.$errors, ...v$.card.expireYear.$errors][0]
                .$message
            }}
          </div>
        </div>
      </div>
      <div class="row">
        <h4>Terms and Conditions</h4>
        <div class="input-group">
          <input type="checkbox" id="terms" v-model="acceptTerms" />
          <label for="terms"
            >I accept the terms and conditions for signing up to this service,
            and hereby confirm I have read the privacy policy.</label
          >
        </div>
        <div class="error" v-if="v$.acceptTerms.$error">
          You must accept the terms and conditions
        </div>
      </div>
      <div class="row">
        <button>save</button>
      </div>
    </form>
  </div>
</template>

<script>
import useVuelidate from "@vuelidate/core";
import {
  required,
  email,
  minLength,
  integer,
  alphaNum,
  sameAs,
  numeric,
} from "@vuelidate/validators";

export default {
  name: "SubscribeForm",
  setup() {
    return {
      v$: useVuelidate({ $autoDirty: true }),
    };
  },
  data() {
    return {
      fullName: "",
      email: "",
      password: "",
      birth: {
        day: "",
        month: "",
        year: "",
      },
      gender: "",
      card: {
        number: "",
        cvc: "",
        expireMonth: "",
        expireYear: "",
      },
      acceptTerms: false,
    };
  },
  validations() {
    return {
      fullName: { required, minLength: minLength(6) },
      email: { required, email },
      password: { required, alphaNum, minLength: minLength(6) },
      birth: {
        day: { required, integer },
        month: { required, integer },
        year: { required, integer },
      },
      gender: { required },
      card: {
        number: {
          required,
          integer,
          minLength: minLength(16),
        },
        cvc: {
          required,
          numeric,
          minLength: minLength(3),
        },
        expireMonth: { required },
        expireYear: { required },
      },
      acceptTerms: { sameAs: sameAs(true) },
    };
  },
  methods: {
    onSubmit() {
      this.v$.$validate();

      if (this.v$.$error) {
        return false;
      }

      console.log("Formulaire validé");
    },
  },
};
</script>

<style>
@import "https://maxcdn.bootstrapcdn.com/font-awesome/4.3.0/css/font-awesome.min.css";

*,
*:before,
*:after {
  box-sizing: border-box;
}
body {
  padding: 1em;
  font-family: "Open Sans", "Helvetica Neue", Helvetica, Arial, sans-serif;
  font-size: 15px;
  color: #b9b9b9;
  background-color: #e3e3e3;
}
h4 {
  color: #7ed321;
}
input,
button,
input[type="radio"] + label,
input[type="checkbox"] + label:before,
select option,
select {
  width: 100%;
  padding: 1em;
  line-height: 1.4;
  background-color: #f9f9f9;
  border: 1px solid #e5e5e5;
  border-radius: 3px;
  -webkit-transition: 0.35s ease-in-out;
  -moz-transition: 0.35s ease-in-out;
  -o-transition: 0.35s ease-in-out;
  transition: 0.35s ease-in-out;
  transition: all 0.35s ease-in-out;
}
input:focus {
  outline: 0;
  border-color: #64ac15;
}
input:focus + .input-icon i {
  color: #7ed321;
}
input:focus + .input-icon:after {
  border-right-color: #7ed321;
}
input[type="radio"] {
  display: none;
}
input[type="radio"] + label,
select {
  display: inline-block;
  width: 50%;
  text-align: center;
  float: left;
  border-radius: 0;
}
input[type="radio"] + label:first-of-type {
  border-top-left-radius: 3px;
  border-bottom-left-radius: 3px;
}
input[type="radio"] + label:last-of-type {
  border-top-right-radius: 3px;
  border-bottom-right-radius: 3px;
}
input[type="radio"] + label i {
  padding-right: 0.4em;
}
input[type="radio"]:checked + label,
input:checked + label:before,
select:focus,
select:active {
  background-color: #7ed321;
  color: #fff;
  border-color: #64ac15;
}
input[type="checkbox"] {
  display: none;
}
input[type="checkbox"] + label {
  position: relative;
  display: block;
  padding-left: 1.6em;
}
input[type="checkbox"] + label:before {
  position: absolute;
  top: 0.2em;
  left: 0;
  display: block;
  width: 1em;
  height: 1em;
  padding: 0;
  content: "";
}
input[type="checkbox"] + label:after {
  position: absolute;
  top: 0.45em;
  left: 0.2em;
  font-size: 0.8em;
  color: #fff;
  opacity: 0;
  font-family: FontAwesome;
  content: "\f00c";
}
input:checked + label:after {
  opacity: 1;
}
select {
  height: 3.4em;
  line-height: 2;
}
select:first-of-type {
  border-top-left-radius: 3px;
  border-bottom-left-radius: 3px;
}
select:last-of-type {
  border-top-right-radius: 3px;
  border-bottom-right-radius: 3px;
}
select:focus,
select:active {
  outline: 0;
}
select option {
  background-color: #7ed321;
  color: #fff;
}
.input-group {
  margin-bottom: 1em;
  zoom: 1;
}
.input-group:before,
.input-group:after {
  content: "";
  display: table;
}
.input-group:after {
  clear: both;
}
.input-group-icon {
  position: relative;
}
.input-group-icon input {
  padding-left: 4.4em;
}
.input-group-icon .input-icon {
  position: absolute;
  top: 0;
  left: 0;
  width: 3.4em;
  height: 3.4em;
  line-height: 3.4em;
  text-align: center;
  pointer-events: none;
}
.input-group-icon .input-icon:after {
  position: absolute;
  top: 0.6em;
  bottom: 0.6em;
  left: 3.4em;
  display: block;
  border-right: 1px solid #e5e5e5;
  content: "";
  -webkit-transition: 0.35s ease-in-out;
  -moz-transition: 0.35s ease-in-out;
  -o-transition: 0.35s ease-in-out;
  transition: 0.35s ease-in-out;
  transition: all 0.35s ease-in-out;
}
.input-group-icon .input-icon i {
  -webkit-transition: 0.35s ease-in-out;
  -moz-transition: 0.35s ease-in-out;
  -o-transition: 0.35s ease-in-out;
  transition: 0.35s ease-in-out;
  transition: all 0.35s ease-in-out;
}
.container {
  max-width: 38em;
  padding: 1em 3em 2em 3em;
  margin: 0em auto;
  background-color: #fff;
  border-radius: 4.2px;
  box-shadow: 0px 3px 10px -2px rgba(0, 0, 0, 0.2);
}
.row {
  zoom: 1;
}
.row:before,
.row:after {
  content: "";
  display: table;
}
.row:after {
  clear: both;
}
.col-half {
  padding-right: 10px;
  float: left;
  width: 50%;
}
.col-half:last-of-type {
  padding-right: 0;
}
.col-third {
  padding-right: 10px;
  float: left;
  width: 33.33333333%;
}
.col-third:last-of-type {
  padding-right: 0;
}

.error {
  color: red;
}

@media only screen and (max-width: 540px) {
  .col-half {
    width: 100%;
    padding-right: 0;
  }
}
</style>