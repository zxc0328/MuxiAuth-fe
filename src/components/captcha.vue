<template>
    <div>
        <div class="row-line full-width">
            <div class="iconbox inline-block full-height vertical-align">
                <svg viewBox="0 0 200 200" class="vertical-align icon-size">
                    <use xmlns:xlink="http://www.w3.org/2000/svg" xlink:href="#email"></use>
                </svg>
            </div>
            <eInput v-model.trim="emailInput" class="transparent inline-block vertical-align inputword"></eInput>
            <div class="inline-block">
                <sendcode :start='start' @countDown='start=false' @click.native='sendCode'></sendcode>
            </div>
        </div>
        <div class="height">
            <div v-if="$v.emailInput.email && $v.emailInput.required && $v.emailInput.isUnique" class="check">您输入的账号不存在，请重新输入
            </div>
            <div v-if="!$v.emailInput.email" class="check">邮箱格式有误</div>
        </div>
        <div class="row-line full-width">
            <div class="iconbox inline-block full-height vertical-align">
                <svg viewBox="0 0 200 200" class="vertical-align icon-size">
                    <use xmlns:xlink="http://www.w3.org/2000/svg" xlink:href="#captcha"></use>
                </svg>
            </div>
            <input type="text" v-model.trim="captchaInput" class="transparent inline-block vertical-align inputword" placeholder="输入验证码">
        </div>
        <button v-on:click="next" class="btn next vertical-align" :style="{'background-color': this.code && this.captchaInput ? '#fd860e':'grey' }">下一步</button>
    </div>
</template>
<script>
import sendcode from './sendcode.vue'
import Input from './Input.vue'
import {
    email,
    required,
    isUnique
} from 'vuelidate/lib/validators'
export default {
    data() {
            return {
                emailInput: '',
                captchaInput: '',
                start: false,
                code: false,
                correct: false
            }
        },
        components: {
            "eInput": Input,
            "sendcode": sendcode
        },
        validations: {
            emailInput: {
                email,
                required,
                isUnique(value) {
                    return new Promise((resolve, reject) => {
                        resolve(typeof value === 'string' &&
                            this.checkemail(value))
                    })
                }
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
            sendCode(value) {
                // value.stopPropagation()
                // value.preventDefault();
                if (this.$v.emailInput) {
                    fetch("/api/forgot_password/get_captcha/", {
                        method: 'POST',
                        headers: {
                            'Accept': 'application/json',
                            'Content-Type': 'application/json'
                        },
                        body: JSON.stringify({
                            email: this.emailInput
                        })
                    }).then(res => {
                        if (res.ok) {
                            this.start = true
                            this.code = true
                        }
                    })
                }
            },
            next() {
                if (this.code && this.captchaInput) {
                    fetch("/api/forgot_password/reset/", {
                        method: 'POST',
                        headers: {
                            'Accept': 'application/json',
                            'Content-Type': 'application/json'
                        },
                        body: JSON.stringify({
                            email: this.emailInput,
                            captcha: this.captchaInput
                        })
                    }).then(res => {
                        if (res.ok) {
                            this.$router.push(reset)
                        }
                    })
                }
            }
        }
}
</script>
<style>
.height {
    height: 30px;
}

.icon-size {
    width: 25px;
    height: 25px;
    transform: translateX(50%);
}

.next {
    margin-top: 20px;
    float: right;
}
</style>
