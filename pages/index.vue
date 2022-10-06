<template>
  <div class="page-container">
    <div class="container">
      <h1 class="title">Add favorite Book</h1>
      <!-- MOVIE INPUT FORM -->
      <form class="form">
        <div class="form-group">
          <label for="movie-name">Book</label>
          <input id="movie-name" type="text" placeholder="Book name" />
        </div>
        <div class="form-group">
          <label for="author">Author</label>
          <input id="author" type="text" placeholder="Author name" />
        </div>
        <div class="form-group">
          <label for="released">Released</label>
          <input id="released" type="text" placeholder="Released date" />
        </div>
        <div>
          <input class="btn" type="submit" value="Add In Your List" />
        </div>
      </form>

      <div class="table-container">
        <table class="table-box">
          <tr>
            <th>Book</th>
            <th>Author</th>
            <th>Released</th>
            <th>x</th>
          </tr>
          <tr
            v-for="allTodoList in allTodoLists"
            :key="allTodoList.id"
            class="org-item"
          >
            <td>{{ allTodoList.bookName }}</td>
            <td>{{ allTodoList.author }}</td>
            <td>{{ allTodoList.released }}</td>
            <td>x</td>
          </tr>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      allTodoLists: [],
    }
  },
  mounted() {
    this.fetchAllData()
  },
  methods: {
    async fetchAllData() {
      try {
        const { data } = await this.$axios.get('http://localhost:3001/todo/all')
        this.allTodoLists = data.todos
        console.log(data)
      } catch (error) {
        alert(error)
      }
    },
  },
}
</script>

<style lang="scss" scoped>
@import '~/styles/style.scss';
.page-container {
  padding: 2rem 2rem;
  background: #f1f1f9;
  min-height: 100vh;
}

/* MAIN WORK PLACE  */
.container {
  width: 90%;
  max-width: 1200px;
  margin: auto;
  .title {
    font-weight: 700;
    font-size: 2.5rem;
    text-transform: capitalize;
    color: #004b63;
    margin-bottom: 1.6rem;
  }
  /* FORM */
  .form-group > label {
    display: block;
    margin-bottom: 0.3rem;
    font-size: 1.3rem;
    color: #004b63;
    font-weight: 500;
  }

  .form-group input {
    // width: 100%;
    // border-radius: 5px;
    // border: 2px solid rgb(216, 215, 216);
    // background-color: #f0eeef;
    // padding: 0.5rem;
    // height: 3rem;
    // margin-bottom: 1rem;
    // outline: none;
    // font-size: 1.2rem;
    // color: #163055;
    // padding: 0.5rem 0.75rem;
    border-radius: 0.35rem;
    box-shadow: none;
    border: 1.5px solid rgba(rgba(0, 75, 99, 1), 0.5);
    font-weight: 400;
    font-family: $font-primary;
    font-size: 1rem;
    color: rgba(0, 75, 99, 1);
    background: transparent;
    width: 100%;

    &:focus {
      border: 1.5px solid rgba(rgba(0, 75, 99, 1), 0.8);
      outline: none;
      box-shadow: inset 0 0 0 0.5px rgba(rgba(0, 75, 99, 1), 0.8);
    }

    &::-webkit-input-placeholder {
      color: rgba(rgba(0, 75, 99, 1), 0.5);
    }

    &:disabled {
      background: rgba(rgba(0, 75, 99, 1), 0.1);
    }
  }
  .form-group {
    margin-bottom: 2rem;
  }

  .btn {
    width: 100%;
    padding: 0.6rem 1.2rem;
    border-radius: 5px;
    transition: 0.3s;
    font-family: $font-primary;
    text-transform: capitalize;
    cursor: pointer;
    font-weight: 500;
    font-size: 1rem;
    border: none;
    color: #ffffff;
    background: rgba(14, 107, 137, 1);
    &:hover {
      background: rgb(4, 74, 97);
    }

    &:disabled {
      background: #bae4f2;
    }
  }

  /* MESSAGE COLOR */
  .success,
  .error {
    color: #fff;
    padding: 10px;
    margin: -7px 0 20px 0;
    border-radius: 5px;
  }

  .success {
    background-color: green;
  }

  .error {
    background-color: red;
  }
  /* CREATOR */
  .creator {
    font-size: 0.7rem;
    text-align: center;
    color: #7f8b9c;
  }
}

.table-container {
  box-shadow: 0px 4px 10px 0px rgba(0, 0, 0, 0.05);
  border-radius: 5px;
  background: $color-primary;
  overflow: hidden;
  margin: 2rem 0;
  border-bottom-right-radius: 0;
  border-bottom-left-radius: 0;
  .table-box {
    border-collapse: collapse;
    width: 100%;
    .org-item {
      background: $color-primary;
      border-top: 1px solid #abeaff;
    }
    .chat-btn {
      display: flex;
      justify-content: end;
    }
    .chat-icon {
      display: flex;
      justify-content: end;
      margin-right: 1rem;
    }
  }
  .table-box td,
  .table-box th {
    padding: 0.75rem 2rem;
  }

  .table-box th {
    text-align: left;
    color: $color-primary;
    background: #1b8fb4;
  }
  .table-box td {
    font-size: 1rem;
    font-weight: 500;
    font-family: $font-primary;
    color: #0e6b89;
  }
}
</style>
