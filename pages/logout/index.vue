<template>
  <div>
    <h3>tunggu sebentar.....</h3>
  </div>
</template>

<script setup>
const supabase = useSupabaseClient();

async function logout() {
  const { error } = await supabase.auth.signOut();
  if (!error) navigateTo("/login");
}

onMounted(() => {
  logout();
});
</script>