<template>
    <div class="wrap">
        <div class="no-footer">
            <div class="header text-align full-width">
                <svg class="logo" viewBox="0 0 200 200">
                    <use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#logo"></use>
                </svg>
            </div>
            <div class="main">
                <div class="top">
                    <div class="circle margin"></div>
                </div>
                <div class="bottom full-width">
                    <div class="container margin">
                        <div class="text-align router-margin">
                            <router-link to="/phone" class="btn">
                                <a v-on:click="tologin" v-bind:class="{highlight: login}">登录</a>
                            </router-link>
                            <router-link to="/register" class="registerButton btn">
                                <a v-on:click="toregister" v-bind:class="{ highlight: !login}">注册</a>
                            </router-link>
                        </div>
                        <router-view></router-view>
                    </div>
                </div>
            </div>
        </div>
        <div class="footer-copy full-width">
            <div class="footer full-width">
                <a href="http://xueer.muxixyz.com" class="word word-change text-align">学而</a>
                <a href="http://muxistudio.com" class="word word-change text-align">木犀团队</a>
                <a href="http://share.muxixyz.com/" class="word word-change text-align">木犀分享</a>
            </div>
            <div class="full-width">
                <div class="copyright text-align">
                    华中师范大学木犀团队
                </div>
                <div class="copyright text-align">
                    Hello from Wuhan，2014-2017 MuxiStudio
                </div>
            </div>
        </div>
    </div>
</template>
<script>
import Login from './login.vue'
import Register from './register.vue'
export default {
    data() {
            return {
                login: true
            }
        },
        components: {
            "login": Login,
            "register": Register
        },
        methods: {
            tologin: function() {
                this.login = true
            },
            toregister: function() {
                this.login = false
            }
        }
}
</script>
<style lang="sass">
@import '../mobile.scss';

.router-margin{
    margin-bottom: 10px;
}

.highlight {
    color: black;
}

.btn {
    font-size: 14px;
    border: none;
    color: grey;
}

.registerButton {
    margin-left: 20px;
}

.header {
    background-color: #f9f5d3;
    height: 37px;
}

.logo {
    width: 180px;
    height: 100px;
    margin-top: -32px;
}

.main {
    width: 100%;
    height: 100%;
}

.top {
    height: 40%;
    padding-top: 10px;
    padding-bottom: 20px;
}

.circle {
    display: block;
    width: 160px;
    height: 160px;
    background-repeat: no-repeat;
    background-size: contain;
    background-position: 50%;
    background-image: url('../pictures/mobile.png');
}

.bottom {
    height: 60%;
}

.container {
    font-size: 18px;
    color: rgba(71, 70, 68, 0.99);
    width: 72%;
}

.psd-height {
    height: 34px;
}

.box-height {
    height: 34px;
}

.inputbox-width {
    width: 66%;
}

.eye {
    float: right;
    width: 10%;
}

.margin-bottom {
    margin-bottom: 100px;
}

.footer-copy {
    background-color: #2b2b29;
    height: 60px;
    padding-top: 3px;
}

.copyright {
    font-size: 11px;
    color: #989f9d;
}

.footer {
    height: 25px;
    display: flex;
}

.word {
    flex: 1;
    line-height: 25px;
    color: #989f9d;
}

.word_change:hover {
    color: #fe850b;
}
</style>
