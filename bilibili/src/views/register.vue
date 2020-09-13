<template>
<div>
    <login-top style="color:#fb7299" Text="注册">
        <div slot="right" style="font-size:3.611vw" @click="$router.push('/login')">跳转登录</div>
    </login-top>
    <login-text label="昵称" style="margin:4.167vw 0;" placeholder="请输入昵称" @contentWatch="res => model.name = res">
    </login-text>

    <login-text label="账号" placeholder="请输入账号" @contentWatch="res => model.username = res">
    </login-text>

    <login-text label="密码" placeholder="请输入密码" type="password" @contentWatch="res => model.password = res">
    </login-text>

    <login-btn BtnText="注册" @TextClick="AjaxInsert"></login-btn>
</div>
</template>

<script>
import LoginTop from '@/components/common/LoginTop.vue'
import LoginText from '@/components/common/LoginText.vue'
import LoginBtn from '@/components/common/LoginBtn.vue'
export default {
    data() {
        return {
            model: {}
        }
    },
    components: {
        LoginTop,
        LoginText,
        LoginBtn
    },
    methods: {
        async AjaxInsert() {
            let rulg = /^.{6,16}/
            if (this.model.name != '' && rulg.test(this.model.username) && rulg.test(this.model.password)) {
                const res = await this.$http.post('/register', this.model)
                this.$msg.fail(res.data.msg)
                localStorage.setItem('token', res.data.objtoken)
                localStorage.setItem('id', res.data.id)
                setTimeout(() => {
                    this.$router.push('/userinfo')
                }, 2000)
            } else {
                this.$msg.fail('格式不正确,请重新输入!')
            }
        }
    }
}
</script>

<style lang="less">

</style>
