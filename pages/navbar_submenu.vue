<template>
<div class="bg">
    <div class="content" style="height: 500px" ref="content">
        <div class="navbar" ref="navbar">
            <!-- looping list menu dari Array navbar -->
            <nav class="navbar-item" v-for="(data, i) in navbar"
                :key="i"
                @mouseover="submenuHover(i)"
                @mouseout="submenuBlur(i)">
                <!-- label menu -->
                {{ data.label }}
                <!-- looping list submenu dengan filter
                jika submenu ditemukan -->
                <i class="bi bi-chevron-down"
                    v-show="data.submenu != null"
                    ref="submenuArrow"></i>
                <div class="submenu hidden" v-show="data.submenu != null"
                    ref="submenuItem">
                    <div class="submenu-item" v-for="(submenu, subIndex) in data.submenu"
                        :key="subIndex">
                        <!-- label submenu -->
                        {{ submenu.label }}
                    </div>
                </div>
            </nav>
        </div>
        <div class="content-body">
            <h2>Newest Product</h2>
            <div class="products-content">
                <!-- mengambil data products di json contentData -->
                <div class="products-item" v-for="(data, i) in contentData" :key="i">
                    <div class="thumb">
                        <img :src="data.thumbnail" alt="">
                    </div>
                    <div class="info">
                        {{ data.title }}
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>
</template>

<script>
// install axios terlebih dahulu
import axios from 'axios';

export default {
    data() {
        return {
            // Variable untuk menampung API
            contentData: [],
            // Array Navbar Menu
            navbar: [{
                    label: 'Home',
                    link: '#'
                },
                {
                    label: 'Community',
                    link: '#',
                    submenu: [{
                        label: 'Forum',
                        link: '#'
                    }, {
                        label: 'Customer Service',
                        link: '#'
                    }]
                },
                {
                    label: 'Pricing',
                    link: '#'
                }, {
                    label: 'Contact',
                    link: '#',
                    submenu: [{
                        label: 'About Us',
                        link: '#'
                    }, {
                        label: 'Careers',
                        link: '#'
                    }, {
                        label: 'Blog',
                        link: '#'
                    }]
                },
            ]
        }
    },
    methods: {
        // Methods jika navbar hover
        submenuHover(i) {
            this.$refs.submenuArrow[i].classList.toggle('bi-chevron-up')
            this.$refs.submenuItem[i].classList.toggle('hidden')
        },
        // Methods jika navbar blur
        submenuBlur(i) {
            this.$refs.submenuArrow[i].classList.toggle('bi-chevron-up')
            this.$refs.submenuItem[i].classList.toggle('hidden')
        }
    },
    async mounted() {
        let content = this.$refs.content;
        content.addEventListener('scroll', () => {
            let navbar = this.$refs.navbar;
            if (content.scrollTop > 0) {
                navbar.classList.add('navbar-anim');
            } else {
                navbar.classList.remove('navbar-anim');
            }
        });
        // Hit API dari dummyjson untuk Products
        await axios.get('https://dummyjson.com/products')
            .then(response => {
                this.contentData = response.data.products;
            })
            .catch(error => {
                console.log(error);
            });
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
    padding: 10px 15px 10px 15px;
    display: flex;
    gap: 5px;
    cursor: pointer;
    background-color: #fff;
}

.navbar-anim {
    box-shadow: 0px 2px 4px rgba(0, 0, 0, 0.1);
    animation: navbarAnimation 0.5s ease-in-out;
}

@keyframes navbarAnimation {
    from {
        transform: translateY(-100%);
    }

    to {
        transform: translateY(0);
    }
}

.navbar-item {
    display: flex;
    gap: 10px;
    align-items: center;
    padding: 5px 15px 5px 15px;
    position: relative;
    font-size: 14px;
}

.navbar-item:hover {
    background-color: #f5f5f5;
}

.navbar-item .submenu {
    position: absolute;
    top: 100%;
    left: 0;
    right: 0;
    background-color: #fff;
    display: flex;
    flex-direction: column;
    border-radius: 5px;
    box-shadow: 0px 2px 4px rgba(0, 0, 0, 0.1);
}

.navbar-item .submenu.hidden {
    display: none;
}

.navbar-item .submenu .submenu-item {
    cursor: pointer;
    font-size: 12px;
    padding: 10px;
}

.navbar-item .submenu .submenu-item:hover {
    background-color: #f5f5f5;
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

.products-content {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(120px, 1fr));
    gap: 20px;
}

.products-content .products-item {
    border-radius: 14px;
    box-shadow: 0px 2px 4px rgba(0, 0, 0, 0.1);
    width: 100%;
    height: 200px;
    display: flex;
    flex-direction: column;
    overflow: hidden;
    font-size: 12px;
    align-items: center;
}

.products-content .products-item .thumb {
    width: 100%;
    height: 130px;
}

.products-content .products-item .thumb img {
    width: 100%;
    height: 100%;
    object-fit: cover;
}

.products-content .products-item .info {
    display: flex;
    flex-direction: column;
    gap: 5px;
    text-align: center;
    padding: 10px;
}
</style>
