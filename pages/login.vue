<template>
    <div>
        <div class="background-container">
            <img src="../assets/image/login-bg.png" class="background-image"/>
        </div>
        <div class="login-box">
            <div class="card p-3 box-shadow w-30 h-370 br-14">
                <div class="brand-container mt-2">
                    <div class="brand-logo"><img src="../assets/image/Shape.svg"></div>
                    <div class="brand-name pl-1">SuperHub</div>
                </div>
                <div class="login-container mt-5">
                    <div class="email-container text-left p-3">
                        <label>Email</label><br>
                        <input class="input-rounded" type="email" autocomplete="off" placeholder="email@example.com" required v-model="email">
                    </div>
                    <div class="password-container text-left p-3">
                        <label>Senha</label><br>
                        <input class="input-rounded" type="password" autocomplete="off" placeholder="senha" required v-model="password">
                    </div>
                    <a class="btn btn-login mt-4" @click.prevent="login()">Entrar</a>
                </div>
            </div>
        </div>
        <Toast v-if="show" :Title="Title" :Desc="Desc" :Type="Type" />
    </div>
</template>
<script>
import axios from 'axios'
import Toast from '@/components/Toast.vue'
import Cookies from 'js-cookie'
export default{
    components: { Toast },
    data(){
        return{
            email: '',
            password: '',
            show: false,
            Title: '',
            Desc: '',
            Type: '',
            loading: false,
            token: ''

        }
    },
    methods:{
        async login(){
            const data = {
                email: this.email,
                password: this.password
            }
            await axios.post(`http://127.0.0.1:8000/api/login`, data,{
                headers:{
                    "Content-Type": 'application/json'
                }
            }).then((response)  =>  {
                this.token = response.data.token
                Cookies.set('token', this.token);
                this.show = true
                this.loading = false
                this.Title = 'YaY!'
                this.Desc = 'Logged in successfully!'
                this.Type = 'S'
                setTimeout(() => {
                this.show = false
                window.location.href = 'http://localhost:3000/'
                }, 2000);
                
            }).catch((error)    =>  {
                console.error(error)
                this.loading = false
                this.Title = 'Oops!'
                this.Desc = 'An error has ocurred!'
                this.Type = 'D'
                this.show = true
                setTimeout(() => {
                this.show = false
                }, 2000);
            })
        }
    }
}
</script>