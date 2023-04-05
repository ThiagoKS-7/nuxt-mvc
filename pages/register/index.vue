<template>
    <div class="register_wrapper">
        <div class="absolute top-5 right-0">
            <ThemeSwitch/>
        </div>
        <h1 class="text-[5em] mb-[0.3em]">Register</h1>
        <div  @submit="registerUser()">
            <div class="form_input">
                <svg :fill="$colorMode.value === 'light' ? 'black' : 'white'"  width="40" height="40" viewBox="0 0 32 32" version="1.1" xmlns="http://www.w3.org/2000/svg">
                <path d="M4 28q0 0.832 0.576 1.44t1.44 0.576h20q0.8 0 1.408-0.576t0.576-1.44q0-1.44-0.672-2.912t-1.76-2.624-2.496-2.144-2.88-1.504q1.76-1.088 2.784-2.912t1.024-3.904v-1.984q0-3.328-2.336-5.664t-5.664-2.336-5.664 2.336-2.336 5.664v1.984q0 2.112 1.024 3.904t2.784 2.912q-1.504 0.544-2.88 1.504t-2.496 2.144-1.76 2.624-0.672 2.912z"></path>
                </svg>
                <input :disabled="loading" type="text" v-model="form.username"  required class="ml-3 rounded-[0.5em] w-[90%] text-black h-[34px]" placeholder="   Enter your username">
            </div>
            <div class="form_input">
                <svg xmlns="http://www.w3.org/2000/svg" 
                height="40" viewBox="0 150 960 960" :fill="$colorMode.value === 'light' ? 'black' : 'white'" 
                width="40">
                    <path d="M140 896q-24 0-42-18t-18-42V316q0-24 18-42t42-18h680q24 0 42 18t18 42v520q0 24-18 42t-42 18H140Zm340-302L140 371v465h680V371L480 594Zm0-60 336-218H145l335 218ZM140 371v-55 520-465Z"/>
                </svg>
                <input :disabled="loading" type="email" v-model="form.email"  required class="ml-3 rounded-[0.5em] w-[90%] text-black h-[34px]" placeholder="   Enter your email">
            </div>
            <div class="form_input">
                <svg xmlns="http://www.w3.org/2000/svg"  
                height="40" 
                viewBox="0 150 960 960" :fill="$colorMode.value === 'light' ? 'black' : 'white'" 
                width="40">
                    <path d="M140 896q-24 0-42-18t-18-42V316q0-24 18-42t42-18h680q24 0 42 18t18 42v520q0 24-18 42t-42 18H140Zm127.909-194Q277 702 283 696.065T289 681V472q0-9-6.5-15.5T266 450q-4.08 0-8.84 1.562-4.76 1.563-8.16 3.438l-49 33q-7 5-8.5 13t3.5 15q5 7 12 8.5t14-3.5l26-17v177q0 9.13 5.909 15.065 5.909 5.935 15 5.935Zm136.706 0h118.893q8.492 0 13.992-5.7T543 683q0-7.6-5.7-13.3-5.7-5.7-13.3-5.7h-90v-2q14-12 30.5-26.5T494 608q21-20 33-38.5t12-44.5q0-31.985-22-53.493Q495 450 460 450q-23 0-40.5 8.414Q402 466.829 391 481q-6 8-3 16.5t13 13.5q6.087 3 13.043 1.5Q421 511 428 505q7-6 14.986-9.5 7.985-3.5 16.585-3.5Q478 492 488 502t10 24q0 20-10 33.5T461 588q-20 19-35.5 33.5T390 654q-5 4-7.5 10.477Q380 670.955 380 677q0 11 7 18t17.615 7ZM694 702q36 0 59.5-17.5T777 630q0-25-13-41t-34-18.4v-.6q19-6 28.5-19.5T768 515q0-30-21.284-47.5T693 450q-16.941 0-32.471 7Q645 464 632 478q-6 7-4 16t11.059 14Q644 510 650 509.5q6-.5 11-5.5 7-6 15-9.5t17.053-3.5q14.71 0 24.329 8.5Q727 508 727 523q0 17-11.667 23-11.666 6-30.333 6-7 0-10.5 5.7T671 571q0 8 3.5 14t11.214 6Q705 591 720.5 601t15.5 29.185Q736 648 724.5 657.5T694 667q-11 0-22.5-6T652 644q-5-6-11-8t-12 0q-8 3-11.5 10.667-3.5 7.666.5 15.333 11 19 29.5 29.5T694 702ZM140 836h680V316H140v520Zm0 0V316v520Z"/>
                </svg>
                <input :disabled="loading" type="password" v-model="form.password" required class="ml-3 rounded-[0.5em] w-[90%] text-black h-[34px]" placeholder="  Enter your password">
            </div>
            <div class="form_input">
                <span class="flex items-center">SuperUser: </span>
                <label class="switch">
                    <input type="checkbox" :disabled="loading" v-model="form.superuser"> 
                    <span class="slider round"></span>
                </label>
            </div>
            <div class="flex justify-end mt-[3em]">
                <button :disabled="loading" class="submit-button" @click="registerUser" >
                    register
                </button>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'registerComponent',
    data() {
        return {
            form: {
                username:"",
                email: "",
                superuser: false,
                password: "",
            },
            loading: false
        }
    },
    methods: {
        async registerUser() {
            try {
                this.loading = true;
                const body = {
                    username: this.form.username,
                    is_superuser: this.form.superuser,
                    email: this.form.email,
                    password: this.form.password
                }
                const token = window.localStorage.getItem("token");
                if (token) {
                    this.$axios.setToken(token, 'Bearer');
                }
                await this.$axios.$post("/users/new",body)
                alert("User created successfully!")
                setTimeout(()=> this.$router.push("/"), 500)

            } catch (e) {
                console.error(e);
            } finally {
                this.loading = false
            }
        },
    }

}
</script>