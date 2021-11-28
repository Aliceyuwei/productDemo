<template>
    <Navbar></Navbar>
    <div class="container-fluid">
      <router-view/>
    </div>
</template>

<script>
import Navbar from '../components/Navbar.vue'

export default {
  components: {
    Navbar
  },
  created () {
    const token = document.cookie.replace(/(?:(?:^|.*;\s*)hexToken\s*=\s*([^;]*).*$)|^.*$/, '$1')
    this.$http.defaults.headers.common.Authorization = `${token}`
    const api = `${process.env.VUE_APP_API}api/user/check`
    this.$http.post(api).then((res) => {
      console.log(res, 'res')
      if (!res.data.success) {
        // 若登入失敗 重新跳轉至燈入頁
        this.$router.push('/login')
      }
    })
  }
}
</script>
