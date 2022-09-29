<template>
  <div class="container">
    <div class="section -red">
      <p>{{ errorText }}</p>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, onBeforeMount, ref } from '@nuxtjs/composition-api'
import axios, { AxiosRequestConfig, AxiosResponse, AxiosError } from 'axios'

export default defineComponent({
  name: 'index',
  setup() {
    const path = 'http://localhost:8000/api/index3'
    const errorText = ref('')
    const response = ref('')

    // @note エラー処理の比較
    const fetchErrorList = async () => {
      const res3 = await axios.get(path).catch((e) => e)
      // console.log(res3)

      if (axios.isAxiosError(res3) && res3?.response?.status !== 200) {
        const error = '取得に失敗しました。'
        console.error(errorText)
        console.warn(errorText)
        console.log(errorText)
        errorText.value = error
        return
      }

      // if (axios.isAxiosError(res3) && res3?.response?.status === 422) {
      //   throw new Error('正常にリクエストできませんでした')
      // }

      // if (axios.isAxiosError(res3) && res3?.response?.status === 401) {
      //   throw new Error('認証に失敗しました。正しいパスワードを入れてください')
      // }

      // if (axios.isAxiosError(res3) && res3?.response?.status === 403) {
      //   throw new Error('もう一度ログインしてからページを閲覧してください。')
      // }

      // if (axios.isAxiosError(res3) && res3?.response?.status === 404) {
      //   throw new Error('アクセスしたページが存在しません。')
      // }
    }

    const fetchApi = async () => {
      try {
        const response = await axios.get(path)
        return response.data
      } catch (error) {
        if (
          axios.isAxiosError(error) &&
          error.response &&
          error.response.status === 400
        ) {
          console.log('400 Error!!')
        }
      }
    }

    onBeforeMount(async () => await fetchErrorList())
    const fetchIndex2 = async () => {
      try {
        const res = await axios.get(path)
        response.value = res.data
      } catch (err) {
        // if (axios.isAxiosError(err)) {
        //   errorText.value = 'handleApi1がエラーになりました。'
        // }
        //* ここで何もしないとエラーが握り潰された状態になり、原因が
      }
    }

    return {
      errorText,
      response,
    }
  },
})
</script>

<style lang="scss" scoped>
.container {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);

  > .section {
    margin: auto;
    border: solid 1px;
    width: 200px;
    height: 200px;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }

  > .-red {
    color: red;
  }

  > .-blue {
    color: blue;
  }

  > .-yellow {
    color: yellow;
  }

  > .-green {
    color: green;
  }
}
</style>
