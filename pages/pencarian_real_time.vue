<template>
<div class="bg">
    <div class="content" ref="content" style="height: 500px">
        <div class="navbar">
            <div class="search-box">
                <!-- Jika input terisi akan ditampung di variabel searchInput dengan v-model -->
                <input type="text" placeholder="Mau Cari Film Apa?" v-model="searchInput">
                <i class="bi bi-search search-icon"></i>
                <!-- Kondisi Jika searchInput Tidak Kosong, akan menampilkan Hasil pencarian -->
                <div class="search-result" v-if="searchInput != ''">
                    <span style="font-size: 12px">Hasil Pencarian Dari {{ searchInput }}</span>
                    <!-- menampilkan data dari searchResult -->
                    <div class="result-item" v-for="(data, i) in searchResult" :key="i">
                        <div class="result-thumb">
                            <img :src="data.Images[0]" alt="">
                        </div>
                        <div class="result-info">
                            {{ data.Title }}
                        </div>
                    </div>
                </div>
                <!-- ==================================== -->
            </div>
        </div>
        <!-- binding class jika searchInput terisi akan menambahkan class Overlay di content-body -->
        <div :class="searchInput != '' ? 'content-body content-overlay' : 'content-body'">
            <h2>Movie List</h2>
            <div class="movie-content">
                <!-- mengambil data film di movie_data -->
                <div class="movie-item" v-for="(data, i) in movie_data" :key="i">
                    <div class="thumb">
                        <img :src="data.Images[0]" alt="">
                    </div>
                    <div class="info">
                        {{ data.Title }} ({{ data.Year }})
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>
</template>

<script>
import axios from 'axios';

export default {
    data() {
        return {
            // Variabel searchInput untuk menampung Kata Kunci Pencarian
            searchInput: '',
            // Variabel searchResult untuk menampung Hasil Pencarian
            searchResult: [],
            // Variabel movie_data untuk menampung Data Film dari API
            movie_data: null,
        }
    },
    watch: {
        // Watch searchInput untuk mengecek apakah terdapat kata kunci pencarian atau tidak
        searchInput(val) {
            if (val != '') {
                // Jika terdapat kata kunci pencarian, akan menjalankan fungsi search()
                this.search(val);
            } else {
                // Jika tidak terdapat kata kunci pencarian, akan menghapus hasil pencarian
                this.searchResult = [];
            }
        }
    },
    methods: {
        search(wordKey) {
            // Mencari Kata Kunci Pencarian yang di Input di movie_data menggunakan Filter berdasarkan Title
            // dan menampungnya di searchResult
            this.searchResult = this.movie_data.filter(item => {
                return item.Title.toLowerCase().includes(wordKey.toLowerCase());
            });
        }
    },
    async mounted() {
        // Setelah proses render selesai, akan mengambil data film dari API
        // menggunakan AXIOS
        const json = `https://gist.githubusercontent.com/saniyusuf/406b843afdfb9c6a86e25753fe2761f4/raw/523c324c7fcc36efab8224f9ebb7556c09b69a14/Film.JSON`
        await axios.get(json)
            .then(response => {
                this.movie_data = response.data;
            })
            .catch(error => {
                console.log(error);
            })
            .finally(() => {
                // console.log(this.movie_data)
            })
    }
}
</script>

<style scoped>
.navbar {
    height: 50px;
    width: 100%;
    position: sticky;
    top: 0;
    left: 0;
    right: 0;
    z-index: 1;
    padding: 10px 20px 10px 20px;
    display: flex;
    gap: 30px;
    cursor: pointer;
    background-color: #fff;
    box-shadow: 0px 2px 4px rgba(0, 0, 0, 0.1);
    z-index: 10;
}

.search-box {
    display: flex;
    justify-content: space-around;
    position: relative;
    align-items: center;
    width: 250px;

}

.search-box input[type=text] {
    width: 100%;
    border: none;
    border-radius: 14px;
    background-color: rgb(231, 231, 231);
    padding: 10px;
    font-size: 14px;
    color: rgb(82, 82, 82);
    transition: all 0.5s ease-in-out;
}

.search-box input[type=text]:focus {
    background-color: rgb(213, 213, 213);
}

.search-box .search-icon {
    position: absolute;
    right: 20px;
    color: rgb(82, 82, 82);
}

.search-box .search-result {
    position: absolute;
    top: 100%;
    height: auto;
    left: 0;
    right: 0;
    padding: 10px;
    background-color: #fff;
    border-radius: 14px;
    box-shadow: 0px 2px 4px rgba(0, 0, 0, 0.1);
}

.search-result .result-item {
    display: flex;
    gap: 10px;
    font-size: 12px;
    padding: 10px;
}

.search-result .result-item:hover {
    background-color: #f5f5f5;
}

.search-result .result-thumb {
    width: 100px;
    height: 100%;
    border-radius: 8px;
    overflow: hidden;
    flex: 1;
}

.search-result .result-thumb img {
    width: 100%;
    height: 100%;
    object-fit: cover;
}

.search-result .result-info {
    flex: 1;
}

.content-body {
    padding: 0px 24px 0px 24px;
    position: relative;
}

.content-overlay {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    background-color: rgba(0, 0, 0, 0.5);
    z-index: 5;
}

.movie-content {
    display: flex;
    flex-wrap: wrap;
    gap: 20px;
}

.movie-content .movie-item {
    border-radius: 14px;
    box-shadow: 0px 2px 4px rgba(0, 0, 0, 0.1);
    width: 150px;
    height: 200px;
    display: flex;
    flex-direction: column;
    overflow: hidden;
    font-size: 12px;
    align-items: center;
}

.movie-content .movie-item .thumb {
    width: 100%;
    height: 130px;
}

.movie-content .movie-item .thumb img {
    width: 100%;
    height: 100%;
    object-fit: cover;
}

.movie-content .movie-item .info {
    display: flex;
    flex-direction: column;
    gap: 5px;
    text-align: center;
    padding: 10px;
}
</style>
