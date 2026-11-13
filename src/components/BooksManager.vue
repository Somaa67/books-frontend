<script setup>
import { getBooks, createBook, deleteBook } from "../api/books";
</script>

<script>
export default {
  name: "BooksManager",
  data() {
    return {
      books: [],
      newBook: {
        title: "",
        author: "",
        year: 2000,
      },
    };
  },
  mounted() {
    this.loadBooks();
  },
  methods: {
    async loadBooks() {
      this.books = await getBooks();
    },
    async addBook(book) {
      await createBook(book);
      await this.loadBooks();
    },
    async removeBook(id) {
      await deleteBook(id);
      await this.loadBooks();
    },
    async submitCreateBook() {
      await this.addBook(this.newBook);
    },
  },
};
</script>

<template>
  <main class=""><h1>Könyv kezelo rendszer</h1></main>

  <form
    @submit.prevent="submitCreateBook"
    class="border-2 border-stone-200 p-2 m-2 flex flex-col gap-4"
  >
    <div class="flex flex-col">
      <label for="title" class="">Cim:</label>
      <input
        v-model="newBook.title"
        type="text"
        name="title"
        class="border-2 border-stone-200 p-1"
      />
    </div>

    <div class="flex flex-col">
      <label for="title" class="">Szerző:</label>
      <input
        v-model="newBook.author"
        type="text"
        name="author"
        class="border-2 border-stone-200 p-1"
      />
    </div>

    <div class="flex flex-col">
      <label for="title" class="">Év:</label>
      <input
        v-model="newBook.year"
        type="number"
        name="year"
        class="border-2 border-stone-200 p-1"
      />
    </div>

    <button class="border-2 border-stone-200 p-2" type="submit">
      Létrehozás
    </button>
  </form>

  <table class="">
    <thead class="">
      <tr>
        <th class="">azonsito</th>
        <th class="">cim</th>
        <th class="">szerzo</th>
        <th class="">ev</th>
        <th class="">muveletek</th>
      </tr>
    </thead>
    <tbody class="">
      <tr v-for="book in books" :key="book.id" class="">
        <td class="">{{ book.id }}</td>
        <td class="">{{ book.title }}</td>
        <td class="">{{ book.author }}</td>
        <td class="">{{ book.year }}</td>
        <td class="">
          <button
            class="ml-2 border-2 border-rose-500 p-2 text-rose-800 cursor-pointer"
            type="button"
            @click="removeBook(book.id)"
          >
            Törlés
          </button>
        </td>
      </tr>
    </tbody>
  </table>
</template>
