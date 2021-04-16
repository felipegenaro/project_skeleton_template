<template>
        <div class="page-sign-up">
                <div class="columns"> 
                        <div class="column is-4 is-offset-4">
                                <h1 class="title">SignUp</h1>

                                <form @submit.prevent="submitForm">
                                        <div class="field">
                                                <label>Username</label>
                                                <div class="control">
                                                        <input type="text" class="input" v-model="username">
                                                </div>
                                        </div>
                                        <div class="field">
                                                <label>Password</label>
                                                <div class="control">
                                                        <input type="password" class="input" v-model="password">
                                                </div>
                                        </div>
                                        <div class="field">
                                                <label>Repeat Password</label>
                                                <div class="control">
                                                        <input type="password" class="input" v-model="password2">
                                                </div>
                                        </div>

                                        <div class="notification is-danger" v-if="errors.length">
                                                <p v-for="error in errors" v-bind:key="error">{{ error }}</p>
                                        </div>

                                        <div class="field">
                                                <div class="control">
                                                        <button class="button is-dark">Sign Up</button>
                                                </div>
                                        </div>

                                        <hr>

                                        Or <router-link to="/log-in">Click Here</router-link> to Log In
                                </form>
                        </div>
                </div>
        </div>
</template>

<script>
import axios from 'axios'
import { toast } from 'bulma-toast'

export default {
        name: 'SignUp',
        data() {
                return {
                        username: '',
                        password: '',
                        password2: '',
                        errors: [],
                }
        },
        components: {
      
        },
        mounted() {
                document.title = 'SignUp'
        },
        methods: {
                submitForm() {
                        this.errors = []

                        if (!this.username.trim()) {
                                this.errors.push('Bad Username')
                        }
                        if (!this.password.trim()) {
                                this.errors.push('Bad Password')
                        }
                        if(this.password !== this.password2) {
                                this.errors.push('Password Doesn\'t Match')
                        }

                        if (!this.errors.length) {
                                const formData = {
                                        username: this.username,
                                        password: this.password,
                                }

                                axios.post('http://localhost:8000/api/v1/users/', formData)
                                .then(response => {
                                        toast({
                                                message: 'Account Created, Please Log In',
                                                statusCode: '201',
                                                type: 'is-success',
                                                dismissible: true,
                                                pauseOnHover: true,
                                                duration: 3000,
                                                position: 'bottom-right',
                                        })

                                        this.$router.push('/log-in')
                                })
                                .catch(err => {
                                        if (err.response) {
                                                for (const property in err.response.data) {
                                                        this.errors.push(`${property}: ${err.response.data[property]}`)
                                                }

                                                console.log(JSON.stringify(err.response.data))
                                        } else if (err.message) {
                                                this.errors.push('Something went wrong. Please try again')
                                                
                                                console.log(JSON.stringify(err))
                                        }
                                })
                        }
                },
        }
}
</script>