<template>
  <div>
    <h1>書籍清單</h1>
    <ul>
      <li v-for="book in bookList" :key="book.bookISBN">
        <h2>{{ book.bookName }}</h2>
        <p>作者: {{ book.bookAuthor }}</p>
        <p>簡介: {{ book.bookIntroduction }}</p>
        <button @click="borrowBook(book)">借閱</button>
      </li>
    </ul>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      bookList: [] // 初始化為空數組
    };
  },
  mounted() {
  this.fetchData(); // 在組件被掛載時調用 fetchData 方法
},
  methods: {
    fetchData() {
      axios.post('http://localhost:8081/esunlibrary/book/bookAll') // 將這裡的GET改為POST
        .then(response => {
          // 更新responseData數據屬性
          this.bookList = response.data.bookList;
        })
        .catch(error => {
          console.error(error);
        });
    },
   

  }
};
</script>