<template>
  <div class="container">
    <div class="card">
      <div class="heading">
        <h4 class="title">NYTimes Books</h4>
      </div>
      <div class="search">
        <select
          name="categories"
          id="categories"
          v-model="selectedCategory"
          @change="onCategoryChange()"
        >
          <option
            v-for="option in categories"
            v-bind:value="option.list_name_encoded"
            :key="option.list_name"
          >
            {{ option.list_name }}
          </option>
        </select>
        <input
          type="text"
          class="form-control"
          placeholder="Search by title"
          v-model="searchTerm"
          @keypress.enter="search()"
          :disabled="categorySelected"
        />
      </div>
      <div class="content">
        <div
          v-if="!books || books.length === 0"
          class="error"
          v-show="error.length > 0"
        >
          {{ error }}
        </div>
        <!-- ADD CONTENT HERE -->
        <div v-else>
          <div
            v-for="item in books"
            @click="goToLink(item.book_details[0])"
            :key="item.book_details[0].title"
          >
            <book
              v-bind:title="item.book_details[0].title"
              v-bind:description="item.book_details[0].description"
              v-bind:author="item.book_details[0].author"
            />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Book from "./Book.vue";
import axios from "axios";

export default {
  name: "Books",
  data() {
    return {
      categories: null,
      books: null,
      selectedCategory: null,
      searchTerm: "",
      headerOption: {
        "api-key": "BYvGyvFAAqySgUwwsBVozkhm7tqylFR9",
      },
      categoriesURL: "https://api.nytimes.com/svc/books/v3/lists/names.json",
      bookSearchURL: "https://api.nytimes.com/svc/books/v3/lists.json",
      categorySelected: true,
      error: "",
    };
  },
  components: {
    Book,
  },
  methods: {
    onCategoryChange() {
      if (this.selectedCategory) {
        this.categorySelected = false;
      }
    },
    search() {
      this.error = "";
      if (this.searchTerm.length >= 1) {
        axios
          .get(this.bookSearchURL, {
            params: { ...this.headerOption, list: this.selectedCategory },
          })
          .then((result) => {
            let value = this.searchTerm.toUpperCase();
            this.books = result.data.results.filter((e) =>
              e.book_details[0].title.includes(value)
            );
            if (this.books.length === 0) {
              this.error =
                "No matches for your search criteria, try something else";
            }
          });
      } else {
        this.error = "You need more than one character to search";
      }
    },
    goToLink(item) {
      //with the replacement I'm changing the spaces for + signs for the search
      window.open(`https://google.com/search?q=${item.author}`, "_blank");
    },
  },
  mounted() {
    axios
      .get(this.categoriesURL, {
        params: this.headerOption,
      })
      .then((response) => {
        //get top 10 categories
        this.categories = response.data.results.slice(0, 10);
      });
  },
};
</script>

<style scoped lang="scss">
.container {
  z-index: 1;
  margin: 36px auto;
  max-width: 826px;
}

.card {
  box-shadow: 0 2px 3px rgba(10, 10, 10, 0.1), 0 0 0 1px rgba(10, 10, 10, 0.1);
  padding: 24px;
}

.content {
  flex-basis: 80%;
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  .error {
    background-color: pink;
    border: black 3px solid;
    color: red;
    padding: 5px 15px;
  }
}

.list {
  > li {
    &:not(:last-child) {
      margin-bottom: 18px;
    }
    > a {
      color: #0a5b8c;
      display: block;
      margin-bottom: 6px;
    }

    > span {
      color: rgba(#3b4242, 0.7);
      font-size: 12px;
    }
  }
}

.btn {
  color: #fff;
  cursor: pointer;
  background-color: #117a8b;
  border: 1px solid transparent;
  padding: 6px 12px;
  border-radius: 6px;
  transition: all 0.1s ease-in;
  &:hover {
    background-color: #138496;
    border-color: #117a8b;
  }
}

.heading {
  margin-bottom: 12px;

  .title {
    color: black;
    text-align: center;
    font-size: 30px;
    font-weight: 600;
  }
}
.search {
  margin-bottom: 24px;

  select {
    background-color: transparent;
    border: none;
    border-bottom: 1px solid #ced4da;
    border-radius: 0;
    font-size: 15px;
    padding: 10px;
    margin: 50px;
  }
  .form-control {
    font-size: 15px;
    background-color: transparent;
    border: none;
    border-bottom: 1px solid #ced4da;
    border-radius: 0;
    outline: 0;
    box-shadow: none;
    padding: 6px 0;
    width: 40%;
    display: inline;
  }
}
</style>
