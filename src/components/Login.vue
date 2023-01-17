<template>
    <div id="login">
        <h1>Login</h1>
        <input type="text" name="username" v-model="input.username" placeholder="Username" />
        <input type="password" name="password" v-model="input.password" placeholder="Password" />
        <button type="button" v-on:click="login()">Login</button>
    </div>
</template>

<script>
import axios from 'axios';
export default {
    name: 'Login',

    data() {
        return {
            input: {
                username: "",
                password: ""
            }
        }
    },
    methods: {
        async login() {
            if (this.input.username != "" && this.input.password != "") {
                if (await this.lookForUser(this.input.username, this.input.password)) {
                    this.$emit("authenticated", true);
                    document.cookie = `auth_user=${this.input.username}; Max-Age=300`
                    this.$router.replace({ name: "drive" })
                } else {
                    console.log("The username and / or password is incorrect");
                }
            } else {
                console.log("A username and password must be present");
            }
        },
        async lookForUser(username, password) {
            try {
                const res = await axios.get(`http://localhost:3001/users`);
                if (res.data) {
                    let user = res.data.find(x => x.login === username)
                    if (user && user.password === password) {
                        return true;
                    } else {
                        return false;
                    }
                }
            } catch (e) {
                console.error(e);
            }
        }



    }
}
</script>

<style scoped>
#login {
    width: 500px;
    border: 1px solid #CCCCCC;
    margin: auto;
    margin-top: 200px;
    padding: 20px;
}
</style>
