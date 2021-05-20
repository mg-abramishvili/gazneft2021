<template>
    <div class="wrapper">

        <div v-show="index" class="wrapper-inner">
            <img src="/pdf/index.svg">
        </div>

        <span class="btn1" @click="GoToPage1()"></span>
        <div v-show="page1" class="wrapper-inner">
            <img src="/pdf/1.svg">
            <span @click="GoToHome()" class="btn-home"></span>
        </div>

        <span class="btn2" @click="GoToPage2()"></span>
        <div v-show="page2" class="wrapper-inner">
            <img src="/pdf/2.svg">
            <span @click="GoToHome()" class="btn-home"></span>
        </div>

    </div>
</template>

<script>
    import {
    INACTIVE_USER_TIME_THRESHOLD,
    USER_ACTIVITY_THROTTLER_TIME
    } from "../constants";

    export default {
        props: {
        },
        data() {
            return {
                index: true,
                page1: false,
                page2: false,
                isInactive: false,
                userActivityThrottlerTimeout: null,
                userActivityTimeout: null
            }
        },
        created() {
        },
        mounted() {
        },
        computed: {
        },
        methods: {
            GoToHome() {
                this.page1 = false,
                this.page2 = false,
                this.index = true
            },
            GoToPage1() {
                this.index = false,
                this.page2 = false
                this.page1 = true
            },
            GoToPage2() {
                this.index = false,
                this.page1 = false
                this.page2 = true
            },
            activateActivityTracker() {
                window.addEventListener("click", this.userActivityThrottler);
                window.addEventListener("touchstart", this.userActivityThrottler);
                window.addEventListener("mousemove", this.userActivityThrottler);
                window.addEventListener("scroll", this.userActivityThrottler);
            },
            deactivateActivityTracker() {
                window.removeEventListener("click", this.userActivityThrottler);
                window.removeEventListener("touchstart", this.userActivityThrottler);
                window.removeEventListener("mousemove", this.userActivityThrottler);
                window.removeEventListener("scroll", this.userActivityThrottler);
            },
            resetUserActivityTimeout() {
                clearTimeout(this.userActivityTimeout);
                this.userActivityTimeout = setTimeout(() => {
                    this.userActivityThrottler();
                    this.inactiveUserAction();
                }, INACTIVE_USER_TIME_THRESHOLD);
            },
            userActivityThrottler() {
                if (this.isInactive) {
                    this.isInactive = false;
                }
                if (!this.userActivityThrottlerTimeout) {
                    this.userActivityThrottlerTimeout = setTimeout(() => {
                    this.resetUserActivityTimeout();
                    clearTimeout(this.userActivityThrottlerTimeout);
                    this.userActivityThrottlerTimeout = null;
                    }, USER_ACTIVITY_THROTTLER_TIME);
                }
            },
            inactiveUserAction() {
                this.page1 = false
                this.page2 = false
            }
        },
        beforeMount() {
            this.activateActivityTracker();
            document.oncontextmenu = new Function("return false;");
        },
        beforeDestroy() {
            this.deactivateActivityTracker();
            clearTimeout(this.userActivityTimeout);
            clearTimeout(this.userActivityThrottlerTimeout);
        },
        components: {
        }
    }
</script>
<style>
    @import '../style.css';
</style>