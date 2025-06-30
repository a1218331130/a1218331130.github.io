
<template>
  <van-form @submit="onSubmit">
    <van-cell-group inset>
      <van-field
        v-model="username"
        name="username"
        label="用户名"
        placeholder="用户名"
        :rules="[{ required: true, message: '请填写用户名' }]"
      />
      <van-field
        v-model="password"
        type="password"
        name="password"
        label="密码"
        placeholder="密码"
        :rules="[{ required: true, message: '请填写密码' }]"
      />
    </van-cell-group>
    <div style="margin: 16px">
      <van-button round block type="primary" native-type="submit">
        提交
      </van-button>
    </div>
  </van-form>
</template>
<script setup>
import { ref, onMounted } from "vue";
let showCarNo = ref(false);
defineProps({
  isMain: Boolean,
});
onMounted(() => {
  // const carNo = localStorage.getItem("carNo");
  // if (!carNo) {
  //   debugger;
  //   showCarNo.value = true;
  // } else {
  // }
});
const username = ref("");
const password = ref("");
const onSubmit = (values) => {
  console.log("submit", values);
};
</script>



<style scoped>
.read-the-docs {
  color: #888;
}
</style>
