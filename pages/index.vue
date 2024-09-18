<template>
    <html lang="en">
    <head>
        <meta charset="UTF-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>menu</title>
    </head>
    <body>
        <div class="container-fluid">
            <div class="row my-5">
                <div class="col-lg-6">
                <nuxt-link to="/transaksi/tambah">
                    <div class="card bg-secondary rounded-5">
                    <div class="card-body">
                        <h2 class="font" style="color: white">TRANSAKSI</h2>
                    </div>
                </div>
                </nuxt-link>
            </div>
            <div class="col-lg-6">
                <nuxt-link to="/transaksi">
                    <div class="card bg-secondary rounded-5">
                        <div class="card-body">
                            <h2 class="font" style="color: white">RIWAYAT</h2>
                        </div>
                    </div>
                </nuxt-link>
            </div>
            </div>
        </div>
        <div class="row my-5">
        <div class="col-lg-6">
            <nuxt-link to="/siswa">
            <div class="card bg-secondary rounded-5">
                <div class="card-body">
                    <h2 class="font" style="color: white">DAFTAR SISWA</h2>
                </div>
            </div>
            </nuxt-link>
        </div>
        <div class="col-lg-6">
            <nuxt-link to="/rekap">
            <div class="card bg-secondary rounded-5">
                    <div class="card-body">
                        <h1 class="font" style="color: white">REKAP BULANAN</h1>
                    </div>
            </div>
            </nuxt-link>
        </div>
        </div>
    </body>
    </html>
</template>
<script setup>
const supabase = useSupabaseClient();
</script>
<style scoped>
  * {
    text-decoration: none;
}
.card {
    height: 250px;
    margin-right: 27px;
    box-shadow: 3px 1px 10px #424242;
}
.card.bg-pengunjung {
    margin-top: 5%;
    background-image: url("../assets/img/bg-home-kunjungan.webp");
    background-repeat: no-repeat;
    background-position: center center;
    background-size: cover;
    opacity: 90%;
}
.card.bg-buku {
    margin-top: 5%;
    background-image: url("../assets/img/bg-home-cari-buku.webp");
    background-repeat: no-repeat;
    background-position: center center;
    background-size: cover;
    opacity: 90%;
}
h2 {
    color: black;
    font: arial;
}
h1 {
    color: white;
}
.card.bg-success {
    margin-top: 2%;
}
.card.bg-warning {
    margin-top: 2%;
}
.statistik {
    color: black;
    margin-left: 50px;
}
.font {
    text-align: center;
    margin-top: 80px;
    font-size: 250%;
    color: black;
}
</style>