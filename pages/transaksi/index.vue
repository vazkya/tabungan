<template>
    <div class="container-fluid">
        <div class="row">
            <h2 class="text-center my-4" style="color: black;">RIWAYAT TRANSAKSI</h2>
            
            <div class="col-lg-12">
                <nuxt-link to="/admin">
                    <button type="button" class="btn btn-lg rounded-5 px-5 bg-secondary text-white mb-3">
                        KEMBALI
                    </button>
                </nuxt-link>
                <div class="col-lg-12 mb-3 bg-secondary" style="color: white; border-radius: 5px;">
                    <label for="filter-nama" style="margin-left: 5px;">NAMA : </label><br>
                    <select style="margin-left: 5px;" v-model="selectedNama" @change="filterTransactions">
                        <option value="">Semua</option>
                        <option v-for="nama in uniqueNamas" :key="nama.id" :value="nama.id">{{ nama.nama }}</option>
                    </select><br>
        
                    <label for="filter-bulan" style="margin-left: 5px;">BULAN: </label><br>
                    <select style="margin-left: 5px;" v-model="selectedBulan" @change="filterTransactions">
                        <option value="">Semua</option>
                        <option v-for="bulan in uniqueBulans" :key="bulan.id" :value="bulan.id">{{ bulan.nama }}</option>
                    </select><br>
        
                    <label for="filter-keperluan" style="margin-left: 5px;">KEPERLUAN: </label><br>
                    <select style="margin-left: 5px; margin-bottom: 5px;" v-model="selectedKeperluan" @change="filterTransactions">
                        <option value="">Semua</option>
                        <option v-for="keperluan in uniqueKeperluans" :key="keperluan.id" :value="keperluan.id">{{ keperluan.nama }}</option>
                    </select>
                </div>
                
                <div class="table-responsive">
                    <table class="table table-striped">
                        <thead>
                            <tr>
                                <th>No</th>
                                <th>NAMA</th>
                                <th>TANGGAL</th>
                                <th>BULAN</th>
                                <th>KEPERLUAN</th>
                                <th>JUMLAH</th>
                                <th>AKSI</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr v-for="(visitor, i) in filteredVisitors" :key="visitor.id">
                                <td>{{ i + 1 }}</td>
                                <td>{{ visitor.nama.nama }}</td>
                                <td>{{ visitor.tanggal }}</td>
                                <td>{{ visitor.bulan.nama }}</td>
                                <td>{{ visitor.keperluan.nama }}</td>
                                <td>Rp. {{ visitor.jumlah }}</td>
                                <td>
                                    <button @click="editData(visitor)" class="btn btn-primary btn-sm mb-2">Edit</button><br>
                                    <button @click="deleteTransaction(visitor.id)" class="btn btn-danger btn-sm">Hapus</button>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
const supabase = useSupabaseClient();
const visitors = ref([]);
const filteredVisitors = ref([]);
const selectedNama = ref('');
const selectedBulan = ref('');
const selectedKeperluan = ref('');
const uniqueNamas = ref([]);
const uniqueBulans = ref([]);
const uniqueKeperluans = ref([]);
const isEditing = ref(false);
const selectedTransaction = ref({});


const fetchTransactions = async () => {
    try {
        const { data, error } = await supabase
            .from("transaksi")
            .select(`*, nama(*), bulan(*), keperluan(*)`)
            .order('id', { ascending: false });
        if (error) throw error;
        visitors.value = data;
        filteredVisitors.value = data; 
    } catch (error) {
        console.error("Error fetching transactions:", error.message);
    }
};


const fetchUniqueFilters = async () => {
    try {
        const { data: namaData } = await supabase.from("siswa").select("id, nama");
        uniqueNamas.value = namaData || [];

        const { data: bulanData } = await supabase.from("bulan").select("id, nama");
        uniqueBulans.value = bulanData || [];

        const { data: keperluanData } = await supabase.from("keperluan").select("id, nama");
        uniqueKeperluans.value = keperluanData || [];
    } catch (error) {
        console.error("Error fetching filter options:", error.message);
    }
};


const filterTransactions = () => {
    filteredVisitors.value = visitors.value.filter(visitor => {
        const matchNama = selectedNama.value ? visitor.nama.id === selectedNama.value : true;
        const matchBulan = selectedBulan.value ? visitor.bulan.id === selectedBulan.value : true;
        const matchKeperluan = selectedKeperluan.value ? visitor.keperluan.id === selectedKeperluan.value : true;
        return matchNama && matchBulan && matchKeperluan;
    });
};


const editData = (visitor) => {
    selectedTransaction.value = { ...visitor };
    isEditing.value = true;
};


const updateData = async () => {
    try {
        const { error } = await supabase.from("transaksi").update({
            nama: selectedTransaction.value.nama.id,
            tanggal: selectedTransaction.value.tanggal,
            bulan: selectedTransaction.value.bulan.id,
            keperluan: selectedTransaction.value.keperluan.id,
            jumlah: selectedTransaction.value.jumlah,
        }).eq('id', selectedTransaction.value.id);
    
        if (error) throw error;

        isEditing.value = false;
        await fetchTransactions();
    } catch (error) {
        console.error("Error updating data:", error.message);
    }
};


const deleteTransaction = async (id) => {
    const confirmed = confirm("Apakah Anda yakin ingin menghapus transaksi ini?");
    if (confirmed) {
        try {
            const { error } = await supabase.from('transaksi').delete().eq('id', id);
            if (error) throw error;
            
            visitors.value = visitors.value.filter(visitor => visitor.id !== id);
            filterTransactions(); 
        } catch (error) {
            console.error("Error deleting transaction:", error.message);
        }
    }
};


onMounted(async () => {
    await fetchTransactions();
    await fetchUniqueFilters();
});
</script>

