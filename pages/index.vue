<template>
  <div class="container">
    <button class="section -yellow" @click="handleApi1">await catch</button>
    <button class="section -blue" @click="handleApi2">async await</button>
    <button class="section -red" @click="handleApi3">then catch</button>
    <button class="section -green" @click="handleApi4">Error handling</button>
    <p class="error -red" v-if="errorText">{{ errorText }}</p>
    <p class="succes -blue" v-if="response">{{ response }}</p>
  </div>
</template>

<script lang="ts">
import { defineComponent, onMounted, ref } from '@nuxtjs/composition-api'
import axios, { AxiosRequestConfig, AxiosResponse, AxiosError } from 'axios'

export default defineComponent({
  name: 'index',
  setup() {
    const indexPath = 'http://localhost:8000/api/api'
    const errorText = ref('')
    const response = ref('')

    //* ---------------------------------  1章  -----------------------------------------

    //* 複数の非同期処理をハンドリングする場合や細かくエラーハンドリングを行いたい時など、、
    const handleApi1 = async () => {
      const res: AxiosResponse = await axios.get(indexPath).catch((e) => e)
      if (res.status !== 200) {
        errorText.value = res.data
        return
      }
      response.value = res.data
    }

    //* 流行らせたい書き方
    const handleApi2 = async () => {
      try {
        const res = await axios.get(indexPath)
        response.value = res.data
      } catch (err) {
        errorText.value = err as string
      }
    }

    // @note bat practice
    const handleApi3 = () => {
      axios
        .get(indexPath)
        .then((res) => {
          response.value = res.data
        })
        .catch((err) => {
          errorText.value = err
        })
        .finally(() => {
          // ローディングを終了させる処理など、
        })
    }

    //* ---------------------------------  2章  -----------------------------------------

    // @note 全ての処理を同期的に取得するために
    const handleApi4 = async () => {
      try {
        const res = await axios.get(indexPath)
        await axios.get(indexPath)
        await axios.get(indexPath)
        await axios.get(indexPath)
        response.value = res.data
      } catch (err) {
        errorText.value = err as string
      }
    }

    // @note 全ての処理を待つ必要はない
    const getFirstView = async () => {
      //* 下記の条件で実行した場合はエラーが握り潰されている状態なので、エラーレスポンスを受け取ったとしても受け取ったことを知らせる方法はない
      axios.get(indexPath)
      const res2 = await axios.get(indexPath).catch((e) => e)

      if (res2?.status !== 200) {
        console.error('画像取得に失敗しました')
      }

      const res3 = await axios.get(indexPath).catch((e) => e)

      if (res3?.status !== 200) {
        console.error('ユーザー情報の取得に失敗しました')
      }
    }

    onMounted(async () => {
      await getFirstView()
    })

    //* ---------------------------------  3章  -----------------------------------------
    return {
      handleApi1,
      handleApi2,
      handleApi3,
      handleApi4,
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
    font-size: 30px;
    width: 200px;
    height: 200px;
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
