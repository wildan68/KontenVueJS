<template>
<div class="bg">
    <div class="content" ref="content">
        <div class="navbar" ref="navbar">
            <nav class="navbar-item">
                Home <i class="bi bi-chevron-down"></i>
            </nav>
            <nav class="navbar-item">
                Blog <i class="bi bi-chevron-down"></i>
            </nav>
            <nav class="navbar-item">
                About <i class="bi bi-chevron-down"></i>
            </nav>
        </div>
        <div class="content-box">
            <h2>Popular Posts</h2>
            <div class="post-list">
                <div class="post-content" v-for="(data, i) in post" :key="i">
                    <div class="title"> {{ data.title }} </div>
                    <div class="date">
                        <i class="bi bi-calendar"></i>
                        <span> {{ data.date }} </span>
                    </div>
                    <div class="content-item">
                        <p>
                            {{ data.content }}
                        </p>
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>
</template>

<script>
export default {
    data() {
        return {
            post: [{
                title: 'Cara terhindar dari pinjol',
                date: 'Minggu, 34 Februari 2022',
                content: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit. Donec eget ex euismod, consectetur nisi eu, consectetur nisi.'
            }, {
                title: 'Cara agar tidak jomblo',
                date: 'Minggu, 69 Februari 2022',
                content: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit. Donec eget ex euismod, consectetur nisi eu, consectetur nisi.'
            }]
        }
    },
    mounted() {
        let content = this.$refs.content;
        content.addEventListener('scroll', ()=> {
            let navbar = this.$refs.navbar;
            if (content.scrollTop > 0) {
                navbar.classList.add('navbar-anim');
            } else {
                navbar.classList.remove('navbar-anim');
            }

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
    padding: 10px 20px 10px 20px;
    display: flex;
    gap: 30px;
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
}

.navbar-item:hover {
    background-color: #f5f5f5;
}

.content-box {
    padding: 10px 20px 10px 20px;
}

.post-list {
    display: flex;
    gap: 20px;
    flex-direction: column;
}

.post-content .date {
    display: flex;
    gap: 5px;
    align-items: center;
    font-size: 12px;
    color: #999;
}


.post-content .content-item {
    margin-top: 10px;
    font-size: 12px
}
</style>
