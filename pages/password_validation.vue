<template>
<div class="bg">
    <div class="content" style="padding: 20px" ref="content">
        <div class="content-body">
            <h4 class="rainbow">Password Validasi</h4>
            <label for="password">Masukan Password</label>
            <div class="input-group">
                <i class="bi bi-lock-fill input-icon"></i>
                <input name="password" type="password" v-model="password" ref="password" placeholder="Masukan Password" autocomplete="off">
                <!-- Jika Icon Show Password di click -->
                <i class="bi bi-eye-fill eye" ref="show" @click.prevent="pwShowToggle"></i>
            </div>
            <div class="validation-group">
                <!-- Password bar strength -->
                <span class="strength" ref="strength"></span>
                {{ pwValidationText }}
            </div>
        </div>
    </div>
</div>
</template>

<script>
export default {
    data() {
        return {
            // Variabel Input Password
            password: '',
            // Variabel Password Validation Text
            pwValidationText: '',
            // Variabel Show Password
            pwShow: false,
        }
    },
    methods: {
        // On / Off Lihat Password
        pwShowToggle() {
            this.pwShow = !this.pwShow
            if (this.pwShow) {
                this.$refs.show.classList.add('bi-eye-slash-fill')
                this.$refs.show.classList.remove('bi-eye-fill')
                this.$refs.password.type = 'text'
            } else {
                this.$refs.show.classList.add('bi-eye-fill')
                this.$refs.show.classList.remove('bi-eye-slash-fill')
                this.$refs.password.type = 'password'
            }
        }
    },
    watch: {
        password(val) {
            if (val.length > 3) {
                // Jika password lebih dari 8 karakter
                if (val.length < 8) {
                    this.$refs.strength.classList.remove('strong')
                    this.$refs.strength.classList.remove('medium')
                    this.$refs.strength.classList.add('weak')
                    this.pwValidationText = `Password Lemah,
                    tidak lebih dari 8 karakter`
                }
                // Jika Password ada huruf capital
                else if (val.length > 8 && val.match(/[A-Z]/)) {
                    this.$refs.strength.classList.remove('strong')
                    this.$refs.strength.classList.add('medium')
                    this.$refs.strength.classList.remove('weak')
                    this.pwValidationText = `Password Sedang,
                    lebih dari 8 karakter dan ada huruf capital`

                    // Jika Password lebih dari 8, menggunakan huruf capital
                    // dan simbol unik atau angka
                    // Bisa ditambahkan Simbol lainnya di Filter Match
                    if (val.match(/[!@#$%^&*,-=/><?{}+]/) || val.match(/[0-9]/)) {
                        this.$refs.strength.classList.add('strong')
                        this.$refs.strength.classList.remove('medium')
                        this.$refs.strength.classList.remove('weak')
                        this.pwValidationText = `Password Kuat, lebih dari 8 karakter,
                        ada huruf capital dan simbol unik atau angka`
                    }
                }
            }
            // Jika Password kurang dari 4 karakter 
            else if (val.length > 0 && val.length < 4) {
                this.$refs.strength.classList.remove('strong')
                this.$refs.strength.classList.remove('medium')
                this.$refs.strength.classList.remove('weak')
                this.pwValidationText = 'Password Kurang dari 4 karakter'
            }
            // Jika tidak ada input password
            else {
                this.$refs.strength.classList.remove('strong')
                this.$refs.strength.classList.remove('medium')
                this.$refs.strength.classList.remove('weak')
                this.pwValidationText = ''
            }
        }
    },
}
</script>

<style scoped>
.content-body {
    display: flex;
    flex-direction: column;
    gap: 10px;
    border-radius: 14px;
    border: 1px solid rgb(209, 209, 209);
    padding: 10px;
    height: 90%;
}

.input-group {
    display: flex;
    flex-direction: column;
    gap: 10px;
    position: relative;
    justify-content: center;
}

label {
    font-size: 12px;
}

.input-group input {
    border-radius: 8px;
    border: 1px solid rgb(209, 209, 209);
    padding: 10px 40px 10px 40px;
}

.input-group .input-icon {
    position: absolute;
    left: 10px;
    color: rgb(153, 153, 153);
}

.input-group .eye {
    position: absolute;
    right: 10px;
    color: rgb(153, 153, 153);
    cursor: pointer;
}

.input-group .eye:hover {
    color: rgb(0, 0, 0);
}

.validation-group {
    display: flex;
    gap: 10px;
    flex-direction: column;
    justify-content: center;
    font-size: 12px;
    color: rgb(153, 153, 153);
}

.validation-group .strength {
    display: flex;
    align-items: center;
    height: 10px;
    border-radius: 4px;
}

.validation-group .strength.weak {
    background-color: rgb(255, 0, 0);
    width: 33.3%;
    animation: weakAnim 0.5s ease-in-out;
}

@keyframes weakAnim {
    from {
        width: 0%;
    }

    to {
        width: 33.3%;
    }
}

.validation-group .strength.medium {
    background-color: rgb(255, 165, 0);
    width: 66.6%;
    animation: mediumAnim 0.5s ease-in-out;
}

@keyframes mediumAnim {
    from {
        width: 33.3%;
    }

    to {
        width: 66.6%;
    }
}

.validation-group .strength.strong {
    background-color: rgb(0, 128, 0);
    width: 100%;
    animation: strongAnim 0.5s ease-in-out;
}

@keyframes strongAnim {
    from {
        width: 66.6%;
    }

    to {
        width: 100%;
    }
}
</style>
