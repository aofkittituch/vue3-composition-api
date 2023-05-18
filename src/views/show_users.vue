<template>
  <router-link to="/add">
    <button
      class="bg-blue-600 text-base-100 font-bold w-24 h-10 mb-2 rounded-lg hover:bg-blue-700"
    >
      เพิ่มข้อมูล
    </button>
  </router-link>
  <div class="overflow-x-auto">
    <table class="table w-full">
      <!-- head -->
      <thead>
        <tr>
          <th>id</th>
          <th>username</th>
          <th>name</th>
          <th>tools</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(user, index) in users">
          <th>{{ index + 1 }}</th>
          <td>{{ user.username }}</td>
          <td class="cap">{{ user.fname }} {{ user.lname }}</td>
          <td>
            <router-link :to="`/edit/${user.id}`">
              <button class="btn btn-warning btn-sm mx-2">แก้ไข</button>
            </router-link>
            <button class="btn btn-error btn-sm" @click="delete_user(user.id)">
              ลบ
            </button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup>
//note
//ref ใช้ .value
//reactive ไม่ต้องใช้ .value
import { ref, onMounted } from "vue";
import axios from "axios";

const users = ref({});

const fetch_users = async () => {
  await axios
    .get(`${import.meta.env.VITE_API}/users`)
    .then((response) => {
      users.value = response.data;
      //   console.log(response.data);
    })
    .catch((err) => {
      console.log(err);
    });
};

const delete_user = async (id) => {
  await axios
    .delete(`${import.meta.env.VITE_API}/users/delete`, { data: { "id": id } })
    .then((res) => {
      console.log(res);
      fetch_users();
    })
    .catch((err) => {
      console.log(err);
    });
};

onMounted(() => fetch_users());
</script>

<style scpoed>
.cap {
  text-transform: capitalize;
}
</style>
