<template>
  <div>
    <div>
      <div class="w-1/2 flex justify-end">
        <button
          @click="showAddComp = true"
          class="bg-indigo-800 border-2 text-white rounded-xl m-3 px-1.5 bg-teal-400 px-5 py-2 stroke-red-500"
          color="lightblue"
        >
          Add
        </button>
      </div>
      <div>
        <table>
          <tbody>
            <tr>
              <th>S.No.</th>
              <th>Name</th>
              <th>Age</th>
              <th>Action</th>
            </tr>
            <tr v-for="(item, idx) of data" :key="idx">
              <td>{{ idx + 1 }}</td>
              <td class="text-left">{{ item.name }}</td>
              <td>{{ item.age }}</td>
              <td>
                <button
                  @click="
                    showEdit(item);
                    idnx = idx;
                  "
                  class="border-gray-400 border-2 rounded-xl m-1 px-2 py-1 bg-indigo-500 text-white"
                  color="black"
                >
                  Edit
                </button>
                <button
                  @click="
                    showDeleteComp = true;
                    idnx = idx;
                  "
                  class="border-gray-400 border-2 rounded-xl m-1 px-2 py-1 bg-indigo-500 text-white"
                  color="black"
                >
                  Delete
                </button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>

      <div class="flex justify-evenly">
        <UModal v-model="showAddComp">
          <UCard>
            <Form @data="pusher" @close="showAddComp = false" mode="add" />
          </UCard>
        </UModal>

        <UModal v-model="showEditComp">
          <UCard>
            <Form
              :dum="showi"
              :item="data[idnx]"
              @update="updateItem"
              @close="showEditComp = false"
              mode="edit"
            />
          </UCard>
        </UModal>

        <UModal v-model="showDeleteComp">
          <UCard>
            <Form
              @delete="remove"
              @close="showDeleteComp = false"
              mode="delete"
            />
          </UCard>
        </UModal>
      </div>
    </div>
  </div>
</template>

<script setup>
const showAddComp = ref(false);
const showEditComp = ref(false);
const showDeleteComp = ref(false);
const idnx = ref(0);
const data = ref([]);

// onMounted(() => {
//   const storedData = JSON.parse(localStorage.getItem("myData")) || [];
//   apiData.value = storedData;
//   showAddComp.value = false;
// });

onMounted(() => {
  const storedData = JSON.parse(localStorage.getItem('myData')) || [];
  data.value = storedData;
  showAddComp.value = false;
});

const pusher = (itm) => {
  data.value.push(itm);
  localStorage.setItem('myData', JSON.stringify(data.value))
};

const remove = () => {
  data.value.splice(idnx.value, 1);
  showDeleteComp.value = false;
  localStorage.setItem('myData', JSON.stringify(data.value))
};
const updateItem = (updatedItem) => {
  data.value[idnx.value] = updatedItem;
  console.log(updatedItem,"dhsgk")
  console.log( data.value," data.value")
  showEditComp.value = false;
  localStorage.setItem('myData', JSON.stringify(data.value)) 
 };
const showi = ref({});
const showEdit = (data) => {
  showi.value = data;
  showEditComp.value = true;
};
</script>

<style>
table,
th,
td {
  border: 1px solid black;
  border-collapse: collapse;
}
td {
  padding-right: 100px;
  padding-left: 100px;
  padding-top: 10px;
}
th {
  color: rgb(242, 239, 246);
  background: rgb(70, 4, 168);
  height: 50px;
}
</style>
