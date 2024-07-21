<template>
  <div class="container" id="app">
    <form @submit.prevent="submitForm">
      <div>
        <input class="input" type="email" placeholder="Email" v-model="formData.email" required>
      </div>
      <div>
        <input class="input" type="number" placeholder="number" v-model="formData.number" @input="formatNumber" maxlength="8">
      </div>
      <button class="button" type="submit">Отправить</button>
    </form>
    <h2>Ответ данных с бэка</h2>
    <div v-if="searchResult.length">
      <ul>
        <li v-for="(item, index) in searchResult" :key="index">{{ item.email }} - {{ item.number }}</li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      formData: {
        email: '',
        number: ''
      },
      searchResult: []
    };
  },
  methods: {
    submitForm() {
      fetch('http://localhost:3000/search', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(this.formData)
      })
          .then(response => response.json())
          .then(data => {
            this.searchResult = data;
          });
    },
    formatNumber() {
      this.formData.number = this.formData.number.replace(/\D/g, '').replace(/(\d{2})(?=\d)/g, '$1-');
    }
  }
};
</script>

<style>
.container {
  height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.input {
  padding: 10px;
  margin-bottom: 10px;
  border: none;
  border-bottom: 2px solid #000;
}

.button {
  width: 100%;
  background-color: #000;
  color: white;
  padding: 10px;
  text-transform: uppercase;
  font-weight: 700;
  letter-spacing: 3px;
  cursor: pointer;
  transition: all 0.3s;
}
.button:hover {
  background-color: #fff;
  color: black;
}

h2 {
  font-family: sans-serif;
}
</style>

