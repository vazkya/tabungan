<template>
    <html lang="en">
    <head>
        <meta charset="UTF-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>menu</title>
    </head>
    <body>
    <div class="logout" style="color: black;">
            <nuxt-link to="/logout">
            Logout
            </nuxt-link>
        </div>
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
            <nuxt-link to="/rekap">
            <div class="card bg-secondary rounded-5">
                    <div class="card-body">
                        <h1 class="font" style="color: white">REKAP</h1>
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
    </body>
    </html>
</template>

<script setup>
definePageMeta({
    middleware: 'auth'
});
</script>
<style scoped>
* {
    color: white;
    text-decoration: none;
}
.card {
    height: 250px;
    margin-right: 27px;
    margin-top: 20px;
    box-shadow: 3px 1px 10px #424242;
}
h2 {
    color: black;
    font: arial;
}
h1 {
    color: white;
}
.font {
    text-align: center;
    margin-top: 80px;
    font-size: 250%;
    color: black;
}
.logout{
    margin-top: 25px;
    background-color: rgb(120, 121, 126);
    width: 80px;
    text-align: center;
    border-radius: 20px;
}
</style>