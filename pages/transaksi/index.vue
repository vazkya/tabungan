<template>
    <html lang="en">
        <head>
        <meta charset="UTF-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>menu</title>
    </head>
    <body>
        <div class="container-fluid">
            <div class="row">
                <div class="col-lg-12">
                <h2 class="text-center my-4">RIWAYAT TRANSAKSI</h2>
                <nuxt-link to="../">
                    <button
                        type="submit"
                        class="btn btn-lg rounded-5 px-5 bg-primary text-white"
                        style="float: right; margin-bottom: 15px">
                    KEMBALI
                    </button></nuxt-link>
                    <div class="my-3 text-muted">
                        menampilkan {{ visitors.length }} dari {{ jumlah }}
                    </div>
                    <table class="table">
                        <thead>
                            <tr>
                            <td>#</td>
                            <td>NAMA</td>
                            <td>WAKTU</td>
                            <td>BULAN</td>
                            <td>KEPERLUAN</td>
                            <td>JUMLAH</td>
                            </tr>
                        </thead>
                    <tbody>
                        <tr v-for="(visitor, i) in visitors" :key="i">
                        <td>{{ i + 1 }}</td>
                        <td>{{ visitor.nama.nama }}</td>
                        <td>{{ visitor.tanggal }}, {{ visitor.waktu }}</td>
                        <td>{{ visitor.bulan.nama }}</td>
                        <td>{{ visitor.keperluan.nama }}</td>
                        <td>{{ visitor.jumlah }}</td>
                        </tr>
                    </tbody>
                    </table>
                </div>
            </div>
            </div>
        </body>
    </html>
</template>
<script setup>
const supabase = useSupabaseClient();
const keyword = ref("");
const visitors = ref([]);
const jumlah = ref([]);

const gettransaksi = async () => {
    const { data, error } = await supabase
    .from("transaksi")
    .select(`*, nama(*), bulan(*), keperluan(*)`)
    .ilike('nama.nama', `%${keyword.value}`)
    .order(`id`, {ascending:false})
    if (data) visitors.value = data;
};
const totalTransaksi = async () => {
    const { data, count } = await supabase
    .from("transaksi")
    .select("*", { count: "exact" });
    if (data) jumlah.value = count;
};
onMounted(() => {
    gettransaksi();
    totalTransaksi();
});
</script>