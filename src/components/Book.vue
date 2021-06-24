<template>
  <div class="book">
    <div class="book-img"><img src="@/assets/book.png" alt="" /></div>
    <div class="book-body">
      <div class="book-title"><h2>{{ title }}</h2></div>
      <div class="description"><h3>{{ description }}</h3></div>
      <div class="author"><h5>Author: {{ author }}</h5></div>
    </div>
  </div>
</template>

<script>
export default {
  props: ["title", "description", "author"],
};
</script>

<style scoped lang="scss">
$orange: orange;
.book {
    cursor: pointer;
    display: flex;
    width: 700px;
    margin: 20px 0;
    height: 150px;
    border: 2px solid black;
    border-radius: 10px;
    align-content: center;
    .book-img {
        width: 30%;
        background-color: rgba(221, 140, 49, .8);
        border-top-left-radius: 10px;
        border-bottom-left-radius: 10px;
        display: flex;
        justify-content: center;
        align-items: center;
    }
    .book-body {
        width: 65%;
        display: flex;
        flex-direction: column;
        padding: 4px 10px;
        justify-content: space-between;
        div {
            width: 100%;
        }
        .description {
            text-align: center;
        }
        .author {
            font-style: italic;
            font-size: 13px;
            font-weight: lighter;
            text-align: right;
        }
        .book-title {
            text-align: left;
            font-size: 22px;
            font-weight: bold;
        }
    }
}
</style>