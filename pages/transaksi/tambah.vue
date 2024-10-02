<template>
    <html lang="en">
        <head>
            <meta charset="UT-8" />
            <meta name="viewport" content="width=device-width, initial-scale=1.0" />
            <title>menu</title>
        </head>
        <body>
            <div class="container-fluid">
            <div class="row">
                <div class="col-lg-12">
                    <h2 class="text-center my-4">MARI MENABUNG</h2>
                    <form @submit.prevent="kirimData">
                    <div class="mb-3">
                    <select
                        v-model="form.nama"
                        class="form-control form-control-lg form-select rounded-5">
                        <option value="">NAMA</option>
                        <option v-for="(n, i) in obje" :key="i" :value="n.id">
                            {{ n.nama }}
                        </option>
                    </select>
                </div>

                <div class="mb-3">
                    <select
                        v-model="form.bulan"
                        class="form-control form-control-lg form-select rounded-5">
                    <option value="">BULAN</option>
                    <option v-for="(b, i) in objec" :key="i" :value="b.id">
                        {{ b.nama }}
                    </option>
                </select>
                </div>
                <select
                    v-model="form.keperluan"
                    class="form-control form-control-lg form-select rounded-5">
                    <option value="">KEPERLUAN</option>
                    <option v-for="(k, i) in objectives" :key="i" :value="k.id">
                        {{ k.nama }}
                    </option>
                </select>
                <div class="mb-3"></div>
                <input
                    v-model="form.jumlah"
                    type="text"
                    class="form-control form-control-lg rounded-5"
                    placeholder="jumlah.."/><br />
                <button
                    type="submit"
                    class="btn btn-lg rounded-5 px-5 bg-primary text-white">
                    KIRIM
                </button>
                <nuxt-link to="../">
                    <button
                        type="submit"
                        class="btn btn-lg rounded-5 px-5 bg-secondary text-white"
                        style="float: right">KEMBALI
                    </button></nuxt-link>
                    </form>
                <br />
            </div>
            </div>
        </div>
        </body>
    </html>
</template>

<script setup>
const supabase = useSupabaseClient();
const obje = ref([]);
const objec = ref([]);
const objectives = ref([]);

const form = ref({
nama: "",
jumlah: "",
keperluan: "",
bulan: "",
});

const kirimData = async () => {
    console.log(form.value)
    const { error } = await supabase.from("transaksi").insert([form.value]);
    if (!error) navigateTo("/transaksi");
};
const getNama = async () => {
    const { data, error } = await supabase.from("siswa").select("*");
    if (data) obje.value = data;
};
const getKeperluan = async () => {
    const { data, error } = await supabase.from("keperluan").select("*");
    if (data) objectives.value = data;
};
const getBulan = async () => {
    const { data, error } = await supabase.from("bulan").select("*");
    if (data) objec.value = data;
};

onMounted(() => {
    getKeperluan();
    getBulan();
    getNama();
});
</script>