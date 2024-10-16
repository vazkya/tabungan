<template>
  <div class="container-fluid">
    <div class="row">
      <h2 class="text-center my-4">REKAP DATA PER BULAN</h2>
      <div class="col-lg-12">
        <nuxt-link to="/admin">
          <button
            type="button"
            class="btn btn-lg rounded-5 px-5 bg-secondary text-white"
            style="float: right; margin-bottom: 15px"
          >
            KEMBALI
          </button>
        </nuxt-link>
        <table class="table table-striped">
          <thead>
            <tr>
              <th>#</th>
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
              <td>
                {{
                  rekap.jumlahPemasukan.toLocaleString("id-ID", {
                    style: "currency",
                    currency: "IDR",
                  })
                }}
              </td>
              <td>
                {{
                  rekap.jumlahPengeluaran.toLocaleString("id-ID", {
                    style: "currency",
                    currency: "IDR",
                  })
                }}
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";
const supabase = useSupabaseClient();
const rekapData = ref([]);

const fetchRekapData = async () => {
  try {
    const { data, error } = await supabase
      .from("transaksi")
      .select(
        `
        tanggal,
        bulan (nama),
        keperluan (nama),
        jumlah
      `
      )
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
    console.error(
      "Terjadi kesalahan saat mengambil data rekap:",
      error.message
    );
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