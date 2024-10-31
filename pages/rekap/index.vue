<template>
  <div class="container-fluid">
    <div class="row">
      <h2 class="text-center my-4">REKAP DATA PER BULAN</h2>
      <div class="col-lg-12">
        <nuxt-link to="/admin">
          <button type="button" class="btn btn-lg rounded-5 px-5 bg-secondary text-white mb-3">
            KEMBALI
          </button>
        </nuxt-link>

        <div class="my-3">
          <h4>Jumlah Tabungan Saat Ini: {{ jumlahTabungan.toLocaleString("id-ID", { style: "currency", currency: "IDR" }) }}</h4>
        </div>

        <div class="table-responsive">
          <table class="table table-striped">
            <thead>
              <tr>
                <th>No</th>
                <th>Tahun</th>
                <th>Bulan</th>
                <th>Jumlah Pemasukan</th>
                <th>Jumlah Pengeluaran</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(rekap, index) in rekapData" :key="index">
                <td>{{ index + 1 }}</td>
                <td>{{ rekap.tahun }}</td>
                <td>{{ rekap.bulan }}</td>
                <td>{{ rekap.jumlahPemasukan.toLocaleString("id-ID", { style: "currency", currency: "IDR" }) }}</td>
                <td>{{ rekap.jumlahPengeluaran.toLocaleString("id-ID", { style: "currency", currency: "IDR" }) }}</td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from "vue";
const supabase = useSupabaseClient();
const rekapData = ref([]);

const jumlahTabungan = computed(() => {
  return rekapData.value.reduce((total, rekap) => {
    return total + rekap.jumlahPemasukan - rekap.jumlahPengeluaran;
  }, 0);
});

const fetchRekapData = async () => {
  try {
    const { data, error } = await supabase
      .from("transaksi")
      .select(`tanggal, bulan (nama), keperluan (nama), jumlah`)
      .order("tanggal", { ascending: false });

    if (error) throw error;

    const monthlyData = {};

    data.forEach((transaction) => {
      const date = new Date(transaction.tanggal);
      const year = date.getFullYear();
      const month = transaction.bulan.nama;
      const key = `${year}-${month}`;

      if (!monthlyData[key]) {
        monthlyData[key] = {
          tahun: year,
          bulan: month,
          jumlahPemasukan: 0,
          jumlahPengeluaran: 0,
        };
      }

      if (transaction.keperluan.nama === "menabung") {
        monthlyData[key].jumlahPemasukan += transaction.jumlah;
      } else if (transaction.keperluan.nama === "menarik") {
        monthlyData[key].jumlahPengeluaran += transaction.jumlah;
      }
    });

    rekapData.value = Object.values(monthlyData).sort((a, b) => {
      const dateA = new Date(`${a.tahun}-${getMonthNumber(a.bulan)}-01`);
      const dateB = new Date(`${b.tahun}-${getMonthNumber(b.bulan)}-01`);
      return dateB - dateA;
    });
  } catch (error) {
    console.error("Terjadi kesalahan saat mengambil data rekap:", error.message);
  }
};

const getMonthNumber = (monthName) => {
  const months = [
    "Januari",
    "Februari",
    "Maret",
    "April",
    "Mei",
    "Juni",
    "Juli",
    "Agustus",
    "September",
    "Oktober",
    "November",
    "Desember",
  ];
  return months.indexOf(monthName) + 1;
};

onMounted(() => {
  fetchRekapData();
});
</script>

<style scoped>
.table-responsive {
  overflow-x: auto;
}
h2 {
  color: black;
}
.table-striped th, .table-striped td {
  white-space: nowrap;
}
@media (max-width: 576px) {
  .table-responsive {
    font-size: 12px;
  }
  .btn {
    padding: 8px 12px;
    font-size: 14px;
  }
}

@media (min-width: 992px) {
  .float-lg-end {
    float: right;
  }
}
</style>
