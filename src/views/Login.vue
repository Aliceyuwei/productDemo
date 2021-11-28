<template>
    <div class="container mt-5">
        <form
            class="row justify-content-center"
            @submit.prevent="signIn()"
        >
        <div class="col-md-6">
            <h1 class="h3 mb-3 font-weight-normal">請先登入</h1>
            <div class="mb-2">
            <label for="inputEmail" class="sr-only">Email address</label>
            <input
                id="inputEmail"
                type="email"
                v-model="user.username"
                class="form-control"
                placeholder="Email address"
                required
                autofocus
            />
            </div>
            <div class="mb-2">
            <label for="inputPassword" class="sr-only">Password</label>
            <input
                id="inputPassword"
                type="password"
                v-model="user.password"
                class="form-control"
                placeholder="Password"
                required
            />
            </div>
            <div class="text-end mt-4">
            <button class="btn btn-lg btn-primary btn-block" type="submit">登入</button>
            </div>
        </div>
        </form>
    </div>
</template>

<script>
export default {
  data () {
    return {
      user: {
        username: '',
        password: ''
      }
    }
  },
  methods: {
    signIn () {
      const api = `${process.env.VUE_APP_API}admin/signin`
      const { user } = this
      this.$http.post(api, user).then((res) => {
        if (res.data.success) {
          const { token, expired } = res.data
          document.cookie = `hexToken=${token}; expired=${new Date(expired)}`
          // 若登入成功 跳轉至首頁
          this.$router.push('/dashboard/products')
        }
      })
    }
  }
}
</script>
