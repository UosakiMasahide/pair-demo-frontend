<template>
  <div class="container">
    <button class="section -yellow" @click="handleApi1">await catch</button>
    <button class="section -blue" @click="handleApi2">async await</button>
    <button class="section -red" @click="handleApi3">then catch</button>
    <p class="error -red" v-if="errorText">{{ errorText }}</p>
    <p class="succes -blue" v-if="response">{{ response }}</p>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from '@nuxtjs/composition-api'
import axios from 'axios'

export default defineComponent({
  name: 'index',
  setup() {
    const path = 'http://localhost:8000/api/index1'
    const errorText = ref('')
    const response = ref('')

    //* ---------------------------------  1章  -----------------------------------------

    //* 複数の非同期処理をハンドリングする場合や細かくエラーハンドリングを行いたい時など、、
    const handleApi1 = async () => {
      const res = await axios.get(path).catch((e) => e)

      if (axios.isAxiosError(res) && res?.response?.status !== 200) {
        errorText.value = 'handleApi1がエラーになりました。'
        return
      }
      response.value = res.data
    }

    //* 流行らせたい書き方
    const handleApi2 = async () => {
      try {
        const res = await axios.get(path)
        response.value = res.data
      } catch (err) {
        if (axios.isAxiosError(err)) {
          errorText.value = 'handleApi1がエラーになりました。'
        }
      }
    }

    // @note bat practice
    const handleApi3 = () => {
      axios
        .get(path)
        .then((res) => {
          response.value = res.data
        })
        .catch((err) => {
          errorText.value = err?.response?.status
        })
        .finally(() => {
          // ローディングを終了させる処理など、
        })
    }

    return {
      handleApi1,
      handleApi2,
      handleApi3,
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
