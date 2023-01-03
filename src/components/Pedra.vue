<template>
  <div class="input-container">
    <label for="value">{{ text }}</label>
    <select name="value" id="value" v-model="value">
      <option v-for="value in data" :key="value.id" :value="value.tipo">
        {{ value.tipo }}
      </option>
    </select>
  </div>
</template>

<script>
export default {
  name: "Pedra",
  props: {
    data: Object,
    text: String,
  },
};
</script>

<style scoped>
.input-container {
  display: flex;
  flex-direction: column;
  margin-bottom: 20px;
}

label {
  font-weight: bold;
  margin-bottom: 15px;
  color: #222;
  padding: 5px 10px;
  border-left: 4px solid #fcba03;
}

input,
select {
  padding: 5px 10px;
  width: 300px;
}
</style>
