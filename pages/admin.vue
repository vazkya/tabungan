<template>
    <html lang="en">
        <head>
            <meta charset="UTF-8" />
            <meta name="viewport" content="width=device-width, initial-scale=1.0" />
            <title>Menu</title>
        </head>
        <body>
            <div class="logout bg-secondary">
            <nuxt-link to="/logout">Logout</nuxt-link>
            </div>
            <div class="row my-5">
            <div class="col-lg-4">
                <nuxt-link to="/transaksi/tambah">
                <div class="card custom-card">
                    <div class="card-body">
                    <h2 class="font">TRANSAKSI</h2>
                    </div>
                </div>
                </nuxt-link>
            </div>
            <div class="col-lg-4">
                <nuxt-link to="/rekap">
                <div class="card custom-card">
                    <div class="card-body">
                    <h1 class="font">REKAP</h1>
                    </div>
                </div>
                </nuxt-link>
            </div>
            <div class="col-lg-4">
                <nuxt-link to="/transaksi">
                <div class="card custom-card">
                    <div class="card-body">
                    <h2 class="font">RIWAYAT</h2>
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
.custom-card {
    height: 250px;
    margin-right: 27px;
    margin-top: 20px;
    transition: background-color 0.3s ease, transform 0.3s ease;
    border: 3px solid var(--bs-secondary); 
    box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.15); 
    background-color: transparent;
    border-radius: 15px;
}
.custom-card:hover, .custom-card:active {
    background-color: var(--bs-secondary);
    color: white;
    transform: translateY(-5px); 
    box-shadow: 0px 8px 16px rgba(0, 0, 0, 0.3);
}
.font {
    text-align: center;
    margin-top: 80px;
    font-size: 250%;
    color: var(--bs-secondary);
    transition: color 0.3s ease;
}
.custom-card:hover .font, .custom-card:active .font {
    color: white; 
}
.logout {
    margin-top: 25px;
    background-color: rgb(120, 121, 126);
    width: 80px;
    height: 30px;
    text-align: center;
    border-radius: 20px;
}
</style>