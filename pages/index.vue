<template>
  <div class="container">
    <button class="section -yellow" @click="handleApi1">await catch</button>
    <button class="section -blue" @click="handleApi2">async await</button>
    <button class="section -red" @click="handleApi3">then catch</button>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "@nuxtjs/composition-api";
import axios from "axios";

export default defineComponent({
  name: "index",
  setup() {

    //* 今回提唱したい書き方
    const handleApi1 = async () => {
      const res = await axios.get("http://localhost:8000/api/api").catch(e => e)
      if (res.status !== 200) {
        console.error(res)
        return
      }
      console.log(res)
      const user = res.data
      return user 
    }

    //* 現在の会社内で主流な書き方
    const handleApi2 = async () => {
      try {
        const res = await axios.get("http://localhost:8000/api/api")
        console.log(res)
      } catch (err) {
        console.error(err)
      }
    }

    //* bat practice
    const handleApi3 = () => {
        const myPromise = axios.get("http://localhost:8000/api/api").then(() => {

          //*成功した時の処理

          console.log('then')
        }).catch((e) => {

          //*失敗した時した時の処理

          console.log('catch', e)
        }).finally(() => {

          //* どちらの処理の時も実行する

          console.log('finally')
        });
        console.log(myPromise, 1)
        // const res = await axios.get("http://localhost:8000/api/api")
        // console.log(res.data, 2)
    }
    return {
      handleApi1,
      handleApi2,
      handleApi3
    };
  },
});
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
}
</style>
