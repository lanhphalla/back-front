<template>
  <section>
    <header>
      <h1>My books - {{ getStatus()}}</h1>
    </header>
    <book-form @add-book="addBook"></book-form>
    <ul>
      <book-card 
      v-for="book of books" 
      :key="book.id" 
      :book="book"
      @delete-book="deleteBook"
      ></book-card>
    </ul>
  </section>
</template>

<script>
import axios from "axios";
const API_URL = "http://localhost:8000/api/books"
export default {
  data() {
    return {
      books: [
        {
          id: 1,
          title: "first Book",
          description: "My first book",
        },
        {
          id: 2,
          title: "second Book",
          description: "My second book",
        },
        {
          id: 3,
          title: "Third Book",
          description: "My third book",
        },
      ],
    };
  },
  methods: {
    getStatus() {
      return  process.env.VUE_APP_MODE;
    },
    deleteBook(bookId){
      // 
      axios.delete(API_URL+ "/"+bookId).then(() => {
      this.books = this.books.filter((book) => book.id !== bookId);
    });
    },
    
    addBook(title, description) {
      const newBook = {
        title: title,
        description: description,
      };
      axios.post(API_URL, newBook).then((bookId) => {
        this.books = this.books.filter((book) => book.id !== bookId)
      })
    },
  },
  mounted() {
    axios.get(API_URL).then((response) => {
      this.books = response.data
    });
  },
};
</script>

<style>
:root {
  --main-color: #3a2dfc;
}

* {
  box-sizing: border-box;
}

html {
  font-family: sans-serif;
}

body {
  margin: 0;
}

header {
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
  margin: 3rem auto;
  border-radius: 10px;
  padding: 1rem;
  background-color: var(--main-color);
  color: white;
  text-align: center;
  width: 90%;
  max-width: 40rem;
}

ul {
  margin: 0;
  padding: 0;
  list-style: none;
}

button {
  cursor: pointer;
  border: 1px solid var(--main-color);
  background-color: var(--main-color);
  color: white;
  padding: 0.5rem 1rem;
  box-shadow: 1px 1px 2px rgba(0, 0, 0, 0.26);
}

button:hover,
button:active {
  box-shadow: 1px 2px 6px rgba(0, 0, 0, 0.26);
}
</style>