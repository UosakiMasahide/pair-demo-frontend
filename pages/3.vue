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
    const showPath = 'http://localhost:8000/api/index3'
    const errorText = ref('')
    const response = ref('')

    // @note エラー処理の比較
    const errorList = async () => {
      const res3 = await axios.get(showPath).catch((e) => e)
      console.log(res3)

      if (axios.isAxiosError(res3) && res3?.response?.status !== 200) {
        const errorText = '取得に失敗しました。'
        console.error(errorText)
        console.warn(errorText)
        // errorText.value = errorText
        // throw new Error('正しい値を引数にしてください')
      }
    }

    onBeforeMount(async () => {
      await errorList()
    })

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
