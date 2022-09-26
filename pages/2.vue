<template>
  <div class="container">
    <div class="section -red" v-if="awaitText.text1">
      <p>{{ awaitText.text1 }}</p>
    </div>
    <div class="section -blue" v-if="awaitText.text2">
      <p>{{ awaitText.text2 }}</p>
    </div>
    <div class="section -yellow" v-if="awaitText.text3">
      <p>{{ awaitText.text3 }}</p>
    </div>
    <div class="section -green" v-if="awaitText.text4">
      <p>{{ awaitText.text4 }}</p>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, onBeforeMount, ref } from '@nuxtjs/composition-api'
import axios, { AxiosRequestConfig, AxiosResponse, AxiosError } from 'axios'

export default defineComponent({
  name: 'index',
  setup() {
    const path = 'http://localhost:8000/api/index2'
    const errorText = ref('')
    const awaitText = ref({
      text1: '',
      text2: '',
      text3: '',
      text4: '',
    })

    //* awaitと宣言すると結果を受け取るまで待ち続けてしまうので同期的な処理として扱わないといけないので順番を考える必要がある。
    // @note NG
    const fetchIndex2 = async () => {
      const res1 = await axios.get(path).catch((e) => e)
      const res2 = await axios.get(path).catch((e) => e)
      const res3 = await axios.get(path).catch((e) => e)
      const res4 = await axios.get(path).catch((e) => e)
      awaitText.value.text1 = res1.data.text1
      awaitText.value.text2 = res2.data.text2
      awaitText.value.text3 = res3.data.text3
      awaitText.value.text4 = res4.data.text4
    }

    // @note OK
    const fetchIndex3 = async () => {
      const res1 = await axios.get(path).catch((e) => e)
      awaitText.value.text1 = res1.data.text1
      const res2 = await axios.get(path).catch((e) => e)
      awaitText.value.text2 = res2.data.text2
      const res3 = await axios.get(path).catch((e) => e)
      awaitText.value.text3 = res3.data.text3
      const res4 = await axios.get(path).catch((e) => e)
      awaitText.value.text4 = res4.data.text4
    }

    onBeforeMount(async () => await fetchIndex3())

    return {
      awaitText,
      errorText,
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
  display: flex;

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
    color: orchid;
  }

  > .-green {
    color: green;
  }
}
</style>
