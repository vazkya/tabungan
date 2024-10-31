<template>
    <div class="container-fluid">
        <div class="row">
            <div class="col-12">
                <h2 class="text-center my-4" style="color: black">RIWAYAT SISWA</h2>
                <nuxt-link to="./">
                    <button
                        type="button"
                        class="btn btn-lg rounded-5 px-4 px-md-5 bg-secondary text-white  mb-3"
                    >
                        KEMBALI
                    </button>
                </nuxt-link>
                <div>
                    <h4 class="my-4" style="color: black">JUMLAH TABUNGAN Rp.{{ totalTabungan }}</h4>
                </div>
                <div class="table-responsive">
                    <table class="table table-striped" v-if="transactions.length > 0">
                        <thead>
                            <tr>
                                <th>No</th>
                                <th>TANGGAL</th>
                                <th>BULAN</th>
                                <th>KEPERLUAN</th>
                                <th>JUMLAH</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr v-for="(transaction, i) in transactions" :key="i">
                                <td>{{ i + 1 }}</td>
                                <td>{{ transaction.tanggal }}</td>
                                <td>{{ transaction.bulan.nama }}</td>
                                <td>{{ transaction.keperluan.nama }}</td>
                                <td>Rp. {{ transaction.jumlah }}</td>
                            </tr>
                        </tbody>
                    </table>
                    <div v-else>
                        <p class="text-muted">Tidak ada transaksi ditemukan</p>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
const { params } = useRoute();
const supabase = useSupabaseClient();
const keyword = ref("");
const transactions = ref([]);
const totalTabungan = ref(0);  

const getTransaksi = async () => {
const { data, error } = await supabase
    .from("transaksi")
    .select(`
        id,
        tanggal,
        waktu,
        jumlah,
        bulan (nama),
        keperluan (nama),
        siswa (id, nama)
    `)
    .eq("nama", params.id)
    .ilike("keperluan.nama", `%${keyword.value}%`)
    .order("id", { ascending: false });
    
if (error) {
    console.error("Error fetching transactions:", error.message);
    return;
}

transactions.value = data || [];
const { data: tabunganData, error: tabunganError } = await supabase
    .from("transaksi")
    .select(`
        jumlah,
        keperluan (nama)
    `)
    .eq("nama", params.id);

if (tabunganError) {
    console.error("Error fetching tabungan:", tabunganError.message);
    return;
}
let total = 0;
tabunganData.forEach((trans) => {
    if (trans.keperluan.nama === "menabung") {
    total += trans.jumlah;
    } else if (trans.keperluan.nama === "menarik") {
    total -= trans.jumlah;
    }
});
totalTabungan.value = total;
};

onMounted(() => {
    getTransaksi();
});
</script>

<style scoped>
@media (max-width: 768px) {
    h2 {
        font-size: 1.5rem;
    }
    .btn {
        font-size: 0.9rem;
        padding: 0.6rem 1.2rem;
    }
    table thead th, table tbody td {
        font-size: 0.85rem;
        padding: 0.5rem;
    }
}
</style>
