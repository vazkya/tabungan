<template>
  <div class="login">
    <form @submit.prevent="Login">
      <h1 style="text-align: center;" class="log">LOGIN</h1>
      <input v-model="email" type="email" class="form-control" id="exampleInputEmail1" aria-describedby="emailHelp" placeholder="Enter email">
      <input v-model="password" type="password" class="form-control" id="exampleInputPassword1" placeholder="Password">
      <button type="submit" class="btn">LOG IN</button>
      
      <NuxtLink to="/"> 
      <button type="submit" class="btn">KEMBALI</button>
      </NuxtLink>
    </form>
  </div>
</template>

<script setup>
const supabase = useSupabaseClient()
const email = ref("");
const password = ref("");
const loading = ref (false);
const error = ref("");

const Login = async () => {
  loading.value = true;
  error.value = "";
  const { data, error: loginError} = await supabase.auth.signInWithPassword ({
    email: email.value,
    password: password.value,
  });
  if (data) {
    loading.value = false
    navigateTo('/admin')
  }
}
</script>

<style scoped>
.login {
  margin: 150px auto;
  width: 350px;
  padding: 10px;
  border: 1px solid #ccc;
  background: rgb(172, 172, 179);
  border-radius: 25px;
}
input[type=email], input[type=password] {
  margin: 20px auto;
  width: 90%;
  padding: 10px;
}
input[type=submit] {
  margin: 5px auto;
  float: right;
  padding: 5px;
  width: 90px;
  border: 1px solid #fff;
  color: #fff;
  background: red;
  cursor: pointer;
}
.btn{
  margin-left: 110px;
  background-color: rgb(77, 79, 87);
  color: white;
  width: 100px;
  margin-top: 5px;
  margin-bottom: 20px;
}
.log{
  background-color: rgb(193, 195, 204);
  border-radius: 25px;
  margin-top: 20px;
  margin-bottom: 20px;
  margin-left: 60px;
  width: 200px;
}
</style>