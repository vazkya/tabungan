<template>
    <html lang="en">
    <head>
        <meta charset="UT-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>buku</title>
    </head>
    <body>
        <div class="container-fluid">
        <div class="row">
            <div class="col-lg-12">
            <h2 class="text-center my-4">SISWA</h2>
            <nuxt-link to="../"
                ><button
                type="submit"
                class="btn btn-lg rounded-5 px-5 bg-primary text-white"
                style="float: right; margin-bottom: 15px"
                >
                KEMBALI
                </button></nuxt-link
            >
            <div class="my-3">
                <form @submit.prevent="getSiswa">
                <input
                    v-model="keyword"
                    type="search"
                    class="form-control rounded-5"
                    placeholder="Mau baca apa hari ini?"
                />
                </form>
            </div>
            <div class="my-3 text-muted">
                menampilkan {{ siswa.length }} dari {{ jumlah }} siswa
            </div>
            <div class="row">
                <div v-for="(siswa, i) in siswa" :key="i" class="col-lg-2">
                <div class="card mb-4">
                    <NuxtLink :to="`/siswa/${siswa.id}`">
                    <img :src="siswa" class="cover" alt="cover" />
                    </NuxtLink>
                </div>
                </div>
            </div>
            </div>
        </div>
        </div>
    </body>
    </html>
</template> 
<script setup>
const supabase = useSupabaseClient();
const keyword = ref("");
const siswa = ref([]);
const jumlah = ref([]);
const getSiswa = async () => {
    const { data, error } = await supabase
    .from("siswa")
    .select(`*, nama(*)`)
    if (data) siswa.value = data;
};
const totalMurid = async () => {
    const { data, count } = await supabase
    .from("siswa")
    .select("*", { count: "exact" });
    if (data) jumlah.value = count;
};
onMounted(() => {
    getSiswa();
    totalMurid();
});
</script>
<style scoped>
.cover {
    width: 100%;
    object-fit: cover;
    object-position: 0 30;
}
</style>