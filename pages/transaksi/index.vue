<template>
    <div class="container-fluid">
        <div class="row">
            <h2 class="text-center my-4">RIWAYAT TRANSAKSI</h2>
        <div class="col-lg-12">
    <table class="table table-striped">
      <thead>
        <tr>
          <th>#</th>
          <th>NAMA</th>
          <th>TANGGAL</th>
          <th>BULAN</th>
          <th>KEPERLUAN</th>
          <th>JUMLAH</th>
          <th>AKSI</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(visitor, i) in visitors" :key="visitor.id">
          <td>{{ i + 1 }}</td>
          <td>{{ visitor.nama.nama }}</td>
          <td>{{ visitor.tanggal }}</td>
          <td>{{ visitor.bulan.nama }}</td>
          <td>{{ visitor.keperluan.nama }}</td>
          <td>Rp. {{ visitor.jumlah }}</td>
          <td>
            <button @click="editData(visitor)" class="btn btn-primary">Edit</button>
            <button @click="deleteTransaction(visitor.id)" class="btn btn-danger">Hapus</button>
          </td>
        </tr>
      </tbody>
    </table>

    <div v-if="isEditing" class="modal">
      <div class="modal-content">
        <h3 style="text-align:center;">Edit Transaksi</h3>
        <div>
          <label>Tanggal</label>
          <input type="date" v-model="selectedTransaction.tanggal" />
        </div>
        <div>
          <label>Bulan</label>
          <input v-model="selectedTransaction.bulan.id" />
        </div>
        <div>
          <label>Keperluan</label>
          <input v-model="selectedTransaction.keperluan.id" />
        </div>
        <div>
          <label>Jumlah</label>
          <input v-model="selectedTransaction.jumlah" type="number" />
        </div>
        <button @click="updateData" class="simpan bg-primary">SIMPAN</button>
        <button @click="isEditing = false" class="batal bg-secondary">BATAL</button>
      </div>
    </div>
  </div>
  </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
const supabase = useSupabaseClient();
const visitors = ref([]);
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
    console.log("Fetched transactions:", visitors.value);
  } catch (error) {
    console.error("Error fetching transactions:", error.message);
  }
};

const editData = (visitor) => {
  selectedTransaction.value = { ...visitor };
  console.log("Editing transaction:", selectedTransaction.value);
  isEditing.value = true; // Set to true to show modal
};


const updateData = async () => {
  try {
    console.log("Updating transaction:", selectedTransaction.value);
    const { error } = await supabase.from("transaksi").update({
      nama: selectedTransaction.value.nama.id,
      tanggal: selectedTransaction.value.tanggal,
      bulan: selectedTransaction.value.bulan.id,
      keperluan: selectedTransaction.value.keperluan.id,
      jumlah: selectedTransaction.value.jumlah,
    }).eq('id', selectedTransaction.value.id);

    if (error) throw error;

    isEditing.value = false;
    await fetchTransactions(); // Refresh data
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
    } catch (error) {
      console.error("Error deleting transaction:", error.message);
    }
  }
};

onMounted(() => {
  fetchTransactions();
});
</script>

<style>
.modal {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.5);
    display: flex;
    align-items: center;
    justify-content: center;
}

.modal-content {
    background-color: white; /* Ubah sesuai kebutuhan */
    padding: 20px;
    border-radius: 5px;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    text-align: center; /* Menjaga teks di tengah */
}

.modal-content {
  background-color: white;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}
.simpan{
    margin-top: 5px;
    margin-bottom: 5px;
    width:100px;
    color: white;
}
.batal{
    width:100px;
    color: white;
}
.btn{
    margin-left: 10px;
    margin-top: 5px;
    margin-bottom: 5px;
}
</style>
