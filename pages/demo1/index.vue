<template>
  <div id="app">
    <button @click="showPopup = true">Show Popup</button>
    
    <div v-if="showPopup" class="overlay " @click="showPopup = false">
      <div class="popup" @click.stop>
        <form @submit.prevent="submitForm">
          <label for="title">Title:</label>
          <input type="text" v-model="newCard.title" id="title" required>

          <label for="content">Content:</label>
          <textarea v-model="newCard.content" id="content" required></textarea>

          <button type="submit">Add Card</button>
          <button type="button" @click="showPopup = false">Close</button>
        </form>
      </div>
    </div>
    
    <div class="cards">
      <div v-for="card in cards" :key="card.id" class="card">
        <h3>{{ card.title }}</h3>
        <p>{{ card.content }}</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      showPopup: false,
      newCard: {
        title: '',
        content: ''
      },
      cards: []
    };
  },
  methods: {
    submitForm() {
      this.cards.push({ ...this.newCard, id: Date.now() });
      this.newCard.title = '';
      this.newCard.content = '';
      this.showPopup = false;
    }
  }
}
</script>

<style>
#app {
  font-family: Arial, sans-serif;
  text-align: center;
  margin-top: 20px;
}
.overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0,0,0,0.5);
  
}
.popup {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%) rotate(360deg);
  transform-origin: left top;
  padding: 80px;
  background: white;
  box-shadow: 0 0 10px rgba(0,0,0,0.1);
  width: fit-content;
  height: fit-content;
}
.cards {
  margin-top: 20px;
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
}
.card {
  border: 1px solid #ccc;
  padding: 20px;
  margin: 10px;
  border-radius: 4px;
}
</style>
