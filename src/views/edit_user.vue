<template>
  <div>
    <h1 class="text-xl font-bold mb-2">แก้ไขข้อมูลผู้ใช้</h1>
    <div class="form-control mb-2">
      <label class="input-group">
        <span>Username</span>
        <input
          type="email"
          class="input input-bordered w-full max-w-xs"
          v-model="user.username"
        />
      </label>
    </div>
    <div class="form-control mb-2">
      <label class="input-group">
        <span>Firstname</span>
        <input
          type="text"
          class="input input-bordered w-full max-w-xs"
          v-model="user.fname"
        />
      </label>
    </div>
    <div class="form-control mb-2">
      <label class="input-group">
        <span>Lirstname</span>
        <input
          type="text"
          class="input input-bordered w-full max-w-xs"
          v-model="user.lname"
        />
      </label>
    </div>
    <div class="form-control mb-2">
      <label class="input-group">
        <span>Password</span>
        <input
          type="password"
          class="input input-bordered w-full max-w-xs"
          v-model="user.password"
        />
      </label>
    </div>
    <div class="form-control mb-2">
      <label class="input-group">
        <span>Confirm Password</span>
        <input
          type="password"
          class="input input-bordered w-full max-w-xs"
          v-model="user.confirm_password"
        />
      </label>
    </div>

    <button
      class="bg-sky-500 text-base-100 font-bold w-24 h-10 rounded-md hover:bg-sky-600"
      @click="check()"
    >
      แก้ไข
    </button>
  </div>
</template>

<script setup>
import axios from "axios";
import { ref, onMounted } from "vue";
import { useRoute } from "vue-router";
import router from "../router";

const route = useRoute();

// console.log(route.params.id);

const user = ref({
  id: "",
  password: "",
  confirm_password: "",
  fname: "",
  lname: "",
  username: "",
});

const fetch_single_user = async () => {
  await axios
    .get(`${import.meta.env.VITE_API}/users/${route.params.id}`)
    .then((res) => {
      user.value.password = res.data.user.password;
      user.value.fname = res.data.user.fname;
      user.value.lname = res.data.user.lname;
      user.value.username = res.data.user.username;
    })
    .catch((err) => {
      console.log(err);
    });
};

onMounted(() => {
  fetch_single_user();
});

const edit_user = async () => {
  await axios
    .put(`${import.meta.env.VITE_API}/users/update`, {
      "id": route.params.id,
      "username": user.value.username,
      "password": user.value.confirm_password,
      "fname": user.value.fname,
      "lname": user.value.lname,
    })
    .then((res) => {
      console.log(res);
    })
    .catch((err) => {
      console.log(err);
    });
};

const check = async () => {
  if (user.value.password != user.value.confirm_password) {
    await alert("password dose not match!");
    user.value.password = "";
    user.value.confirm_password = "";
  } else {
    await edit_user();
    await alert("password match!");
    await router.push("/");
  }
};
</script>

<style lang="scss" scoped></style>
