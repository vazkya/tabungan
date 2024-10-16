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
                    <nuxt-link to="/admin">
                        <button
                            type="submit"
                            class="btn btn-lg rounded-5 px-5 bg-secondary text-white"
                            style="float: right; margin-bottom: 15px">
                        KEMBALI
                        </button>
                    </nuxt-link>
                    <div class="my-3 text-muted">
                        Menampilkan {{ visitors.length }} dari {{ jumlah }}
                    </div>
                    <table class="table">
                        <thead>
                            <tr>
                                <td>#</td>
                                <td>NAMA</td>
                                <td>TANGGAL</td>
                                <td>BULAN</td>
                                <td>KEPERLUAN</td>
                                <td>JUMLAH</td>
                                <td>AKSI</td>
                            </tr>
                        </thead>
                        <tbody>
                            <tr v-for="(visitor, i) in visitors" :key="i">
                                <td>{{ i + 1 }}</td>
                                <td>{{ visitor.nama.nama }}</td>
                                <td>{{ visitor.tanggal }}</td>
                                <td>{{ visitor.bulan.nama }}</td>
                                <td>{{ visitor.keperluan.nama }}</td>
                                <td>Rp. {{ visitor.jumlah }}</td>
                                <td>
                                    <button @click="editTransaksi(visitor)" class="btn btn-primary">
                                        Edit
                                    </button>
                                    <button @click="hapusTransaksi(visitor.id)" class="btn btn-danger">
                                        Hapus
                                    </button>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </div>
            </div>
        </div>

        <div v-if="isEditing" class="modal">
            <h3>Edit Transaksi</h3>
            <div>
                <label>Nama</label>
                <input v-model="selectedTransaksi.nama.nama" />
            </div>
            <div>
                <label>Tanggal</label>
                <input type="date" v-model="selectedTransaksi.tanggal" />
            </div>
            <div>
                <label>Bulan</label>
                <input v-model="selectedTransaksi.bulan.nama" />
            </div>
            <div>
                <label>Keperluan</label>
                <input v-model="selectedTransaksi.keperluan.nama" />
            </div>
            <div>
                <label>Jumlah</label>
                <input v-model="selectedTransaksi.jumlah" type="number" />
            </div>
            <button @click="updateTransaksi" class="btn btn-success">Simpan</button>
            <button @click="isEditing = false" class="btn btn-secondary">Batal</button>
        </div>
    </body>
</html>
</template>

<script setup>
import { ref, onMounted } from 'vue';
const supabase = useSupabaseClient();
const keyword = ref("");
const visitors = ref([]);
const jumlah = ref([]);


const isEditing = ref(false);
const selectedTransaksi = ref(null);


const gettransaksi = async () => {
    const { data, error } = await supabase
    .from("transaksi")
    .select(`*, nama(*), bulan(*), keperluan(*)`)
    .ilike('nama.nama', `%${keyword.value}`)
    .order('id', {ascending: false});
    if (data) visitors.value = data;
};


const totalTransaksi = async () => {
    const { count } = await supabase
    .from("transaksi")
    .select("*", { count: "exact" });
    jumlah.value = count;
};


const hapusTransaksi = async (id) => {
    const confirmed = confirm("Apakah Anda yakin ingin menghapus transaksi ini?");
    if (confirmed) {
        const { error } = await supabase
        .from('transaksi')
        .delete()
        .eq('id', id);

        if (!error) {
            visitors.value = visitors.value.filter(visitor => visitor.id !== id);
            totalTransaksi();
        } else {
            console.error("Error menghapus transaksi", error);
        }
    }
};


const editTransaksi = (transaksi) => {
    selectedTransaksi.value = { ...transaksi };
    isEditing.value = true;
};


const updateTransaksi = async () => {
    const { id, nama, tanggal, bulan, keperluan, jumlah } = selectedTransaksi.value;

    const { error } = await supabase
        .from('transaksi')
        .update({
            nama : nama.nama,
            tanggal : tanggal,
            bulan : bulan.nama,
            keperluan : keperluan.nama,
            jumlah : jumlah,
        })
        .eq('id', id);

    if (!error) {
        const index = visitors.value.findIndex(visitor => visitor.id === id);
        if (index !== -1) {
            visitors.value[index] = { ...selectedTransaksi.value };
        }
        isEditing.value = false;
        totalTransaksi();
    } else {
        console.error("Error mengupdate transaksi", error);
    }
};


onMounted(() => {
    gettransaksi();
    totalTransaksi();
});
</script>

<style>
.modal {
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    background-color: white;
    padding: 20px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    z-index: 1000;
}
</style>