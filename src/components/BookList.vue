<template>
  <div>
    <h3>Find a book</h3>
    <input id="term" :placeholder="msg" v-on:keyup.enter="search" />
    <button v-on:click="search">Search</button>
    <div v-if="resultSize">
      {{ resultSize }} books found.
      <BasePagination
        :current-page="currentPage"
        :page-count="pageCount"
        class="book-list__pagination"
        @nextPage="pageChangeHandle('next')"
        @previousPage="pageChangeHandle('previous')"
        @loadPage="pageChangeHandle"
      />
    </div>
    <div id="results">
      <ol v-if="books.length">
        <BookItem v-for="book in books" :key="book.id" :book="book" />
      </ol>
    </div>
    <div v-if="resultSize">
      {{ resultSize }} books found.
      <BasePagination
        :current-page="currentPage"
        :page-count="pageCount"
        class="book-list__pagination"
        @nextPage="pageChangeHandle('next')"
        @previousPage="pageChangeHandle('previous')"
        @loadPage="pageChangeHandle"
      />
    </div>
  </div>
</template>

<script>
import axios from "axios";
import BasePagination from "./BasePagination";
import BookItem from "./BookItem";

export default {
  name: "BookList",
  props: {
    msg: String,
  },
  static: {
    apiUrl: "https://WobblyImpartialSets--five-nine.repl.co/search/",
    visibleItemsPerPageCount: 20,
  },
  data() {
    return {
      books: [],
      resultSize: 0,
      pageCount: 0,
      currentPage: 1,
    };
  },
  components: {
    BasePagination,
    BookItem,
  },
  methods: {
    async search() {
      await this.pageChangeHandle(1);
    },
    async pageChangeHandle(value) {
      const term = document.getElementById("term").value;
      if (!term) {
        return;
      }
      switch (value) {
        case "next":
          this.currentPage += 1;
          break;
        case "previous":
          this.currentPage -= 1;
          break;
        default:
          this.currentPage = value;
      }
      try {
        const { data } = await axios.get(
          `${this.$options.static.apiUrl}${term}?page=${this.currentPage}`
        );
        this.books = data.list;
        this.resultSize = data.total;
        this.pageCount = Math.ceil(
          this.resultSize / this.$options.static.visibleItemsPerPageCount
        );
        // Give each book an id.
        this.books.forEach((item, i) => {
          item.id = i + 1;
        });
      } catch(err){
        this.books = [];
        this.resultSize = 0;
        this.pageCount = 0;
      }
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#term {
  margin-bottom: 16px;
}
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
a {
  color: #42b983;
}
</style>
