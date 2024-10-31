<template>
    <html lang="en">
        <head>
            <meta charset="UTF-8" />
            <meta name="viewport" content="width=device-width, initial-scale=1.0" />
            <title>Menu</title>
        </head>
        <body>
            <div class="row my-5">
            <div class="col-lg-6">
                <nuxt-link to="/siswa">
                <div class="card custom-card">
                    <div class="card-body">
                    <h2 class="font">DAFTAR SISWA</h2>
                    </div>
                </div>
                </nuxt-link>
            </div>
            <div class="col-lg-6">
                <nuxt-link to="/login">
                <div class="card custom-card">
                    <div class="card-body">
                    <h1 class="font">ADMIN</h1>
                    </div>
                </div>
                </nuxt-link>
            </div>
            </div>
        </body>
    </html>
</template>
<style scoped>
  * {
    color: white;
    text-decoration: none;
}
.custom-card {
    height: 250px;
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
    text-align: center;
    border-radius: 20px;
}
</style>