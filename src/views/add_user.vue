<template>
  <div>
    <h1 class="text-xl font-bold mb-2">เพิ่มข้อมูลผู้ใช้</h1>
    <div class="form-control mb-2">
      <label class="input-group">
        <span>Email</span>
        <input
          type="email"
          class="input input-bordered w-full max-w-xs"
          v-model="add.email"
        />
      </label>
    </div>
    <div class="form-control mb-2">
      <label class="input-group">
        <span>Username</span>
        <input
          type="email"
          class="input input-bordered w-full max-w-xs"
          v-model="add.username"
        />
      </label>
    </div>
    <div class="form-control mb-2">
      <label class="input-group">
        <span>Firstname</span>
        <input
          type="text"
          class="input input-bordered w-full max-w-xs"
          v-model="add.fname"
        />
      </label>
    </div>
    <div class="form-control mb-2">
      <label class="input-group">
        <span>Lirstname</span>
        <input
          type="text"
          class="input input-bordered w-full max-w-xs"
          v-model="add.lname"
        />
      </label>
    </div>
    <div class="form-control mb-2">
      <label class="input-group">
        <span>Password</span>
        <input
          type="password"
          class="input input-bordered w-full max-w-xs"
          v-model="add.password"
        />
      </label>
    </div>
    <div class="form-control mb-2">
      <label class="input-group">
        <span>Confirm Password</span>
        <input
          type="password"
          class="input input-bordered w-full max-w-xs"
          v-model="add.confirm_password"
        />
      </label>
    </div>

    <button
      class="bg-sky-500 text-base-100 font-bold w-24 h-10 rounded-md hover:bg-sky-600"
      @click="check()"
    >
      เพิ่ม
    </button>
  </div>
</template>

<script setup>
import { ref } from "vue";
import axios from "axios";
import router from "../router";

const add = ref({
  email: "",
  password: "",
  confirm_password: "",
  fname: "",
  lname: "",
  username: "",
  avatar: "https://www.melivecode.com/users/cat.png",
});

const insert_user = async () => {
  await axios
    .post(`${import.meta.env.VITE_API}/users/create`, {
      "fname": add.value.fname,
      "email": add.value.email,
      "password": add.value.confirm_password,
      "lname": add.value.lname,
      "username": add.value.username,
      "avatar": add.value.avatar,
    })
    .then((response) => {
      console.log(response);
    })
    .catch((err) => {
      console.log(err);
    });
};

const check = async () => {
  if (add.value.password == add.value.confirm_password) {
    await insert_user();
    await alert("password match!");
    await router.push("/");
  } else {
    alert("password dose not match!");
    add.value.confirm_password = "";
    add.value.password = "";
  }
};
</script>

<style lang="scss" scoped></style>
