<!--<template>
    <div class="app-container">
        <div class="app-view">
            <template v-if="$route.matched.length">
                <router-view></router-view>
            </template>
        </div>
    </div>
</template>

<script>
export default {
  computed: {
    loggedIn () {
      return this.$store.state.loggedIn
    }
  }
}
</script>-->

<template>
    <div id="app">
        <router-link to="/" tag="button" id='home-button'> Home </router-link>
        <button v-if='authenticated' v-on:click='logout' id='logout-button'> Logout </button>
        <button v-else v-on:click='$auth.loginRedirect' id='login-button'> Login </button>
        <router-view />
    </div>
</template>

<script>

    export default {
        name: 'app',
        data: function () {
            return {
                authenticated: false
            }
        },
        created() {
            this.isAuthenticated()
        },
        watch: {
            // Everytime the route changes, check for auth status
            '$route': 'isAuthenticated'
        },
        methods: {
            async isAuthenticated() {
                this.authenticated = await this.$auth.isAuthenticated()
            },
            async logout() {
                await this.$auth.logout()
                await this.isAuthenticated()

                // Navigate back to home
                this.$router.push({ path: '/' })
            }
        }
    }
</script>

<style>
    html, body {
        margin: 0;
        padding: 0;
    }

    body {
        font: 14px -apple-system, BlinkMacSystemFont, Helvetica, Arial, sans-serif;
        line-height: 1.4em;
        background: #F3F5F6;
        color: #4d4d4d;
    }

    ul {
        padding: 0;
    }

    h1, h2 {
        text-align: center;
    }

    .app-container {
        display: flex;
        align-items: center;
        justify-content: center;
    }

    .app-view {
        background: #fff;
        min-width: 400px;
        padding: 20px 25px 15px 25px;
        margin: 30px;
        position: relative;
        box-shadow: 0 2px 4px 0 rgba(0, 0, 0, 0.2), 0 5px 10px 0 rgba(0, 0, 0, 0.1);
    }
</style>
