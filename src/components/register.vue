<template>
    <div>
        <div class="row">
            <div class="box box-height transparent">
                <div class="iconbox full-height width inline-block vertical-align">
                    <svg viewBox="0 0 200 200" class="icon">
                        <use xmlns:xlink="http://www.w3.org/2000/svg" xlink:href="#username"></use>
                    </svg>
                </div>
                <userInput v-model.trim="username" @focus="isFocus" class="inputbox transparent inline-block vertical-align"></userInput>
            </div>
            <div v-if="$v.username.required && $v.username.isUnique" class="check tip-style">用户名已注册
            </div>
            <div v-if="!$v.username.maxLength && $v.username.required" class="check tip-style">不超过八个字符
            </div>
        </div>
        <div class="row">
            <div class="box box-height transparent">
                <div class="iconbox full-height width inline-block vertical-align">
                    <svg viewBox="0 0 200 200" class="icon">
                        <use xmlns:xlink="http://www.w3.org/2000/svg" xlink:href="#email"></use>
                    </svg>
                </div>
                <eInput v-model.trim="emailInput" @focus="isFocus" class="inputbox transparent inline-block vertical-align"></eInput>
            </div>
            <div v-if="$v.emailInput.email && $v.emailInput.required && $v.emailInput.isUnique" class="check tip-style">邮箱已注册
            </div>
            <div v-if="!$v.emailInput.email" class="check tip-style">邮箱格式不正确</div>
        </div>
        <div class="row">
            <div class="box box-height transparent">
                <div class="iconbox full-height width inline-block vertical-align">
                    <svg viewBox="0 0 200 200" class="icon">>
                        <use xmlns:xlink="http://www.w3.org/2000/svg" xlink:href="#password"></use>
                    </svg>
                </div>
                <input v-model.trim="passwordInput" @focus="isFocus" type="password" class="inputbox transparent inline-block vertical-align" placeholder="密码(不少于六位)" v-show="!showPass">
                <input v-model.trim="passwordInput" @focus="isFocus" type="text" class="inputbox transparent inline-block vertical-align" placeholder="密码(不少于六位)" v-show="showPass">
                <div class="iconbox full-height eye inline-block vertical-align" v-on:click="showPass = !showPass">
                    <svg viewBox="0 0 200 200" class="icon">
                        <use xmlns:xlink="http://www.w3.org/2000/svg" xlink:href="#eye"></use>
                    </svg>
                </div>
            </div>
            <div class="check tip-style" v-if="!$v.passwordInput.minLength">密码请勿少于六位</div>
        </div>
        <div class="row">
            <div class="box box-height transparent">
                <div class="iconbox full-height width inline-block vertical-align">
                    <svg viewBox="0 0 200 200" class="icon">>
                        <use xmlns:xlink="http://www.w3.org/2000/svg" xlink:href="#password"></use>
                    </svg>
                </div>
                <input v-model.trim="psdsecond" class="inputbox transparent inline-block vertical-align" type="password" placeholder="再次输入密码" v-show="!showPass">
                <input v-model.trim="psdsecond" class="inputbox transparent inline-block vertical-align" type="text" placeholder="再次输入密码" v-show="showPass">
                <div class="iconbox full-height eye inline-block vertical-align" v-on:click="showPass = !showPass">
                    <svg viewBox="0 0 200 200" class="icon">
                        <use xmlns:xlink="http://www.w3.org/2000/svg" xlink:href="#eye"></use>
                    </svg>
                </div>
            </div>
            <div class="check tip-style" v-if="!$v.psdsecond.sameAs && this.psdsecond">密码输入不一致</div>
        </div>
        <button v-on:click="submit" class="change full-width box-height margin-bottom" :style="changedButton" id="registerbtn">注册</button>
    </div>
</template>
<script>
import Input from './Input.vue'
import userInput from './userInput.vue'
import {
    email,
    minLength,
    maxLength,
    sameAs,
    required
} from 'vuelidate/lib/validators'
export default {
    data() {
            return {
                username: '',
                emailInput: '',
                passwordInput: '',
                psdsecond: '',
                focus: false,
                showPass: false,
                submitFlag: false
            }
        },
        components: {
            "eInput": Input,
            "userInput": userInput
        },
        validations: {
            username: {
                maxLength: maxLength(8),
                required,
                isUnique(value) {
                    return new Promise((resolve, reject) => {
                        resolve(typeof value === 'string' &&
                            this.checkUsername(value))
                    })
                }
            },
            emailInput: {
                email,
                required,
                isUnique(value) {
                    return new Promise((resolve, reject) => {
                        resolve(typeof value === 'string' &&
                            this.checkemail(value))
                    })
                }
            },
            passwordInput: {
                minLength: minLength(6),
                required
            },
            psdsecond: {
                sameAs: sameAs('passwordInput'),
                required
            },
            validationGroup: ['username', 'emailInput', 'passwordInput', 'psdsecond']
        },
        computed: {
            changedButton: function() {
                return {
                    'background-color': this.submitFlag ? 'grey' : '#fd860e'
                }
            }
        },
        mounted() {
            if (window.devicePixelRatio && devicePixelRatio >= 2) {
                var boxes = document.querySelectorAll('.box')
                for (var i = 0; i < boxes.length; i++)
                    boxes[i].className += ' box1';
            }
        },
        methods: {
            checkemail(value) {
                fetch(`/api/email_exists/?email=${value}`).then(res => {
                    if (res.ok) {
                        return false
                    } else {
                        return true
                    }
                })
            },
            checkUsername(value) {
                if (this.$v.username.maxLength && this.$v.username.required) {
                    fetch("/api/username_exists/?username=" + this.username, {}).then(res => {
                        if (res.ok) {
                            return false
                        } else {
                            return true
                        }
                    })
                }
            },
            // userBlur() {
            //     this.userblur = true
            //     if (this.$v.username.maxLength && this.$v.username.required) {
            //         fetch("/api/username_exists/?username=" + this.username, {}).then(res => {
            //             if (res.ok) {
            //                 this.username_exist = false
            //             } else {
            //                 this.username_exist = true
            //             }
            //         })
            //     }
            // },
            isFocus() {
                this.submitFlag = false
                this.focus = true
            },
            submit(e) {
                if (this.submitFlag) return
                this.submitFlag = true
                if (this.$v.validationGroup) {
                    fetch("/api/register/", {
                        method: 'POST',
                        headers: {
                            'Accept': 'application/json',
                            'Content-Type': 'application/json'
                        },
                        body: JSON.stringify({
                            username: this.username,
                            email: this.emailInput,
                            password: this.passwordInput
                        })
                    }).then(res => {
                        console.log("success")
                        if (res.ok) {
                            window.location = "/"
                        }
                    })
                }
            }
        }
}
</script>
<style>
.inputbox {
    width: 65%;
}
</style>
