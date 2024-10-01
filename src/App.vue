<script setup>
import { ref } from "vue";

const open = ref(false);

const errorMessage = ref("");

let date = new Date();
// localStorage.clear()
const localNotes = JSON.parse(localStorage.getItem('notes'));

const notes = localNotes ? ref(localNotes) : ref([]);
const newNote = ref('');

const getRandomColor = () => {
  let color = "hsl(" + Math.random() * 360 + ", 100%, 75%)";
  return color
}

const addNote = () => {
  if (newNote.value.length <= 9) {
    return errorMessage.value = "Note needs at least 10 characters"
  }
  notes.value.push(
    {
      id: genId(),
      text: newNote.value,
      date: date.toDateString(),
      backgroundColor: getRandomColor()
    }

  )
  open.value = false;
  newNote.value = '';
  errorMessage.value = '';
  localStorage.setItem('notes', JSON.stringify(notes.value))
};
const genId = () => {
  if (!notes.value.id) {
    return 1;
  }
  let id = Math.max(...notes.value.map(function (note) { return note.id; }))
  return id + 1;

}

</script>
<template>
  <main>
    <div v-if="open" class="overlay">
      <div class="modal">
        <textarea name="note" id="note" v-model.trim="newNote" cols="30" rows="10"></textarea>
        <p style="color:red" v-if="errorMessage">{{ errorMessage }}</p>
        <button @click="addNote">Add Note</button>
        <button @click="open = false" class="close">Close</button>
      </div>
    </div>
    <div class="container">
      <header>
        <h1>Notes</h1>
        <button @click="open = true">+</button>
      </header>
      <div class="cards-container">
        <div v-for="note in notes" :key="note.id" class="card" :style="{ backgroundColor: note.backgroundColor }">
          <p class="main-text">{{ note.text }} </p>
          <p class="date">{{ note.date }}</p>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
main {
  height: 100vh;
  width: 100vw;
}

.container {
  max-width: 1000px;
  padding: 10px;
  margin: 0 auto;
}

header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

h1 {
  font-weight: bold;
  margin-bottom: 25px;
  font-size: 75px;
}

header button {
  border: nono;
  padding: 10px;
  width: 50px;
  height: 50px;
  cursor: pointer;
  background-color: rgb(21, 20, 20);
  border-radius: 100%;
  color: white;
  font-size: 20px;
}

.card {
  width: 255px;
  height: 255px;
  background-color: rgb(237, 182, 44);
  padding: 10px;
  border-radius: 15px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  margin-right: 20px;
  margin-bottom: 20px;
}

.date {
  font-size: 12.5px;
  font-weight: bold;
}

.cards-container {
  display: flex;
  flex-wrap: wrap;
}

.overlay {
  position: absolute;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.77);
  z-index: 10;
  display: flex;
  align-items: center;
  justify-content: center;
}

.modal {
  width: 750px;
  background-color: white;
  border-radius: 10px;
  padding: 30px;
  position: relative;
  display: flex;
  flex-direction: column;

}

.modal button {
  padding: 10px 20px;
  font-size: 20px;
  width: 100%;
  background-color: blueviolet;
  border: none;
  color: white;
  cursor: pointer;
  margin-top: 15px;

}

.modal .close {
  background-color: red;
  margin-top: 7px;
}
</style>
