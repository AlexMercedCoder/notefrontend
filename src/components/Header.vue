<template>

    <b-navbar>
        <template slot="brand">
            <b-navbar-item tag="router-link" :to="{ path: '/' }">
                <span class="logo">notesAPP</span>
            </b-navbar-item>
        </template>
       

        <template slot="end">
            <b-navbar-item tag="div">
                <div class="buttons">
                    <router-link :to="{ name: 'Login', query: { URL: this.URL }}" v-bind:URL="URL" v-if="!loggedIn"><button class="button is-link">Log in</button></router-link>
                    <button v-if="loggedIn" class="button is-link" @click="logout">Logout</button>
                </div>
            </b-navbar-item>
        </template>
    </b-navbar>
</template>

<script>

export default {
    name: 'Header',
    props: ['URL', 'loggedIn'],
    methods: {
        logout: function(){
            this.$emit('logout')
        }
    }
}

</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Barriecito&display=swap');

nav {
width: 80%;
margin: 10px auto;
}

span.logo {
    font-family: Barriecito;
    font-size: 2em;
}

</style>