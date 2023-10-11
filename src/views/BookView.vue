<template>
  <div>
    <h1>書籍清單</h1>
    <table class="center-table">
      <thead>
        <tr>
          <th>書名</th>
          <th>作者</th>
          <th>簡介</th>
          <th>操作</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="book in bookList" :key="book.bookISBN">
          <td>{{ book.bookISBN }}</td>
          <td>{{ book.bookName }}</td>
          <td>{{ book.bookAuthor }}</td>
          <td>{{ book.bookIntroduction }}</td>
          <td><button @click="borrowBook(book)">借閱</button></td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<style>
.center-table {
  margin: 0 auto; /* 设置表格水平居中 */
  width: 80%; /* 可以根据需要调整表格的宽度 */
}
</style>



<script>
import { ref, onMounted } from 'vue';
import axios from 'axios';

export default {
  setup() {
    const bookList = ref([]);

    const fetchData = () => {
      axios.post('http://localhost:8081/esunlibrary/book/bookAll')
        .then(response => {
          // 更新 bookList
          bookList.value = response.data.bookList;
        })
        .catch(error => {
          console.error(error);
        });
    };

    // 在组件被挂载时调用 fetchData 方法
    onMounted(() => {
      fetchData();
    });

    return {
      bookList
    };
  }
};
</script>