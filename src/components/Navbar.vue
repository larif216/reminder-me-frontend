<template>
    <div>
        <b-navbar toggleable="lg">
            <b-navbar-brand href="#">
                <img src="../assets/logo.png" width="30" height="30">
            </b-navbar-brand>

            <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

            <b-collapse id="nav-collapse" is-nav>
                <b-navbar-nav>
                    <b-nav-item href="#">Reminder.me</b-nav-item>
                </b-navbar-nav>
            </b-collapse>
        </b-navbar>
    </div>
</template>

<script>

export default {
    name: 'Navbar',
    data() {
        return {
        }
    },
}
</script>

<style>
    nav {
        background-color: #323F75 !important;
    }

    .navbar-light .navbar-nav .nav-link {
        color: white !important;
    }
</style>