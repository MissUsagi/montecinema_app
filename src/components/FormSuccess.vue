<template>
      <div id="success-panel" class="page-container">
      <h1 class="mb-1">Good job <span>{{ data.name}}!</span></h1>
      <div class="text">
         <p>We have sent you an email to <strong>{{ data.email}} </strong></p>
         <p>Make sure to click the link from the message to activate your account.</p>
      </div>
      <a class="btn btn-primary m1" href="/index.html">Go to homepage</a>
   </div>
</template>


<script>
import RegisterForm from "./RegisterForm.vue";
export default{
   props: {
      data:Object
   },
     components: {
    RegisterForm,
  },}
  </script>

  <style scoped>
  h1 span {
   text-transform: capitalize;
  }
  </style>