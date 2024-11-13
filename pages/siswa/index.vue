<template>
    <div class="container-fluid">
        <div class="row">
            <div class="col-lg-12">
                <h2 class="text-center my-4">DAFTAR SISWA</h2>
                <nuxt-link to="../"
                ><button
                type="submit"
                class="btn btn-lg rounded-5 px-5 bg-secondary text-white"
                style="margin-bottom: 15px"
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
                    placeholder="cari siapa?"
                    />
                </form>
            </div>
            <div class="my-3 text-muted">
                menampilkan {{ siswa.length }} dari {{ jumlah }} siswa
            </div>
            <table class="table table-striped">
                <thead>
                    <tr>
                    <th>No</th>
                    <th>Nama</th>
                </tr>
                </thead>
                <tbody>
                    <tr v-for="(s, i) in siswa" :key="i">
                    <td>{{ i + 1 }}</td>
                    <td>
                        <NuxtLink :to="`/siswa/${s.id}`">{{ s.nama }}</NuxtLink>
                    </td>
                    </tr>
                    </tbody>
            </table>
        </div>
    </div>
</div>
</template>
<script setup>
useHead({title: "siswa", meta: [{nama: "nama", content: "DAFTAR SISWA"}]});
const supabase = useSupabaseClient();
const keyword = ref("");
const siswa = ref([]);
const jumlah = ref([]);

const getSiswa = async () => {
    const { data, error } = await supabase
    .from("siswa")
    .select(`*`)
    .ilike("nama", `%${keyword.value}%`);
    if (data) {
    siswa.value = data;
    console.log(data);
    }
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
*{
    color: black;
    text-decoration: none;
}
.cover {
    width: 100%;
    object-fit: cover;
    object-position: 0 30;
}
</style>
