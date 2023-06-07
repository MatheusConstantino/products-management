<template>
    <div class="page-container">
        <div class="sidebar">
            <div class="brand">
                <img src="../assets/image/Shape.svg" alt="Logo" class="logo">
                <span class="brand-text">SuperHub</span>
            </div>

            <ul class="menu flex-column h-100">
                <a class="btn p-0 w-100">
                    <li :class="{'menu-item': true, 'item-active': true}">
                        <div class="menu-item-content ">
                            <span class="material-icons icon-active">home</span>
                            <p>Home</p>
                        </div>
                    </li>
                </a>
                <a class="btn p-0 w-100">
                    <li :class="{'menu-item': true, 'item-active-logout': true}" @click="logout()">
                        <div class="menu-item-content ">
                            <span class="material-icons icon-active">logout</span>
                            <p>Logout</p>
                        </div>
                    </li>
                </a>
            </ul>
        </div>
    </div>
</template>
  
<script>
import Cookies from 'js-cookie'
export default {
    methods: {
        async logout() {
            Cookies.remove('token')
            window.location.href = 'http://localhost:3000/login'
        },
    }
}
</script>

  