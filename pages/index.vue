<template>
  <div class="page-container">
    <div class="container">
      <h1 class="title">Add favorite Book</h1>
      <form class="form" @submit.prevent>
        <div class="form-group">
          <label for="book">Book</label>
          <input
            id="book"
            v-model="bookName"
            type="text"
            placeholder="Book name"
          />
        </div>
        <div class="form-group">
          <label for="author">Author</label>
          <input
            id="author"
            v-model="authorName"
            type="text"
            placeholder="Author name"
          />
        </div>
        <div class="form-group">
          <label for="released">Released</label>
          <input
            id="released"
            v-model="released"
            type="text"
            placeholder="Released date"
          />
        </div>
        <div>
          <button
            v-if="!isEditing"
            class="btn"
            :disabled="!bookName || !authorName || !released"
            @click.prevent="postItem"
          >
            {{ isPostItem ? 'Adding New List...' : 'Add In Your List' }}
          </button>
          <button v-else class="btn" @click.prevent="updateTodoItem">
            {{ isUpdatingTodo ? 'Updating Todo...' : 'Update Todo' }}
          </button>
        </div>
      </form>

      <div class="table-container">
        <table class="table-box">
          <tr>
            <th>Book</th>
            <th>Author</th>
            <th>Released</th>
            <th>Update</th>
            <th>Delete</th>
          </tr>
          <p v-if="loadingScreener">Loading Book list...</p>
          <tr
            v-for="allTodoList in allTodoLists"
            v-else
            :key="allTodoList.id"
            class="org-item"
          >
            <td>{{ allTodoList.bookName }}</td>
            <td>{{ allTodoList.author }}</td>
            <td>{{ allTodoList.released }}</td>
            <td>
              <button
                class="custom-style"
                @click="updateItem(allTodoList.id, allTodoList)"
              >
                <svg
                  width="14"
                  xmlns="http://www.w3.org/2000/svg"
                  viewBox="0 0 512 512"
                  fill="#1b8fb4"
                >
                  <path
                    d="M421.7 220.3l-11.3 11.3-22.6 22.6-205 205c-6.6 6.6-14.8 11.5-23.8 14.1L30.8 511c-8.4 2.5-17.5 .2-23.7-6.1S-1.5 489.7 1 481.2L38.7 353.1c2.6-9 7.5-17.2 14.1-23.8l205-205 22.6-22.6 11.3-11.3 33.9 33.9 62.1 62.1 33.9 33.9zM96 353.9l-9.3 9.3c-.9 .9-1.6 2.1-2 3.4l-25.3 86 86-25.3c1.3-.4 2.5-1.1 3.4-2l9.3-9.3H112c-8.8 0-16-7.2-16-16V353.9zM453.3 19.3l39.4 39.4c25 25 25 65.5 0 90.5l-14.5 14.5-22.6 22.6-11.3 11.3-33.9-33.9-62.1-62.1L314.3 67.7l11.3-11.3 22.6-22.6 14.5-14.5c25-25 65.5-25 90.5 0z"
                  />
                </svg>
              </button>
            </td>
            <td>
              <button class="custom-style" @click="deleteItem(allTodoList.id)">
                <svg
                  width="14"
                  xmlns="http://www.w3.org/2000/svg"
                  viewBox="0 0 448 512"
                  fill="#1b8fb4"
                >
                  <path
                    d="M135.2 17.7C140.6 6.8 151.7 0 163.8 0H284.2c12.1 0 23.2 6.8 28.6 17.7L320 32h96c17.7 0 32 14.3 32 32s-14.3 32-32 32H32C14.3 96 0 81.7 0 64S14.3 32 32 32h96l7.2-14.3zM32 128H416V448c0 35.3-28.7 64-64 64H96c-35.3 0-64-28.7-64-64V128zm96 64c-8.8 0-16 7.2-16 16V432c0 8.8 7.2 16 16 16s16-7.2 16-16V208c0-8.8-7.2-16-16-16zm96 0c-8.8 0-16 7.2-16 16V432c0 8.8 7.2 16 16 16s16-7.2 16-16V208c0-8.8-7.2-16-16-16zm96 0c-8.8 0-16 7.2-16 16V432c0 8.8 7.2 16 16 16s16-7.2 16-16V208c0-8.8-7.2-16-16-16z"
                  />
                </svg>
              </button>
            </td>
          </tr>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
// const swal = require('sweetalert2')

export default {
  data() {
    return {
      bookName: '',
      authorName: '',
      released: '',
      isPostItem: false,
      isUpdatingTodo: false,
      isEditing: false,
      loadingScreener: false,
      selectedIndex: null,
      allTodoLists: [],
    }
  },
  fetch() {
    this.fetchAllData()
  },

  methods: {
    async fetchAllData() {
      try {
        this.loadingScreener = true
        const { data } = await this.$axios.get(
          'https://zany-rose-alligator-yoke.cyclic.app/todo/all'
        )
        this.allTodoLists = data.todos
        // console.log(data)
      } catch (error) {
        alert(error)
      } finally {
        this.loadingScreener = false
      }
    },

    // Post Api
    async postItem() {
      try {
        this.isPostItem = true
        const { data } = await this.$axios.post(
          'https://zany-rose-alligator-yoke.cyclic.app/todo',
          {
            bookName: this.bookName,
            author: this.authorName,
            released: this.released,
          }
        )
        this.allTodoLists.push({
          bookName: this.bookName,
          author: this.authorName,
          released: this.released,
        })
        this.$swal('Good job!', 'Added New Todo List', 'success')
        this.bookName = ''
        this.authorName = ''
        this.released = ''
        console.log(data)
      } catch (error) {
        console.log(error)
      } finally {
        this.isPostItem = false
      }
    },

    updateItem(id, allTodoList) {
      this.bookName = allTodoList.bookName
      this.authorName = allTodoList.author
      this.released = allTodoList.released
      this.selectedIndex = id
      this.isEditing = true
    },

    // Update  Api
    async updateTodoItem() {
      try {
        this.isUpdatingTodo = true
        const { data } = await this.$axios.put(
          `https://zany-rose-alligator-yoke.cyclic.app/todo/${this.selectedIndex}`,
          {
            bookName: this.bookName,
            author: this.authorName,
            released: this.released,
          }
        )
        this.$swal('Good job!', 'Updated Todo List', 'success')
        this.bookName = ''
        this.authorName = ''
        this.released = ''
        if (data.success === true) {
          this.fetchAllData()
        }
      } catch (err) {
        console.log(err)
      } finally {
        this.isEditing = false
        this.isUpdatingTodo = false
      }
    },

// Delete Api
    deleteItem(id) {
      try {
        this.$swal({
          title: 'Are you sure?',
          text: 'Do you want to delete the content?',
          type: 'warning',
          showCancelButton: true,
          confirmButtonColor: '#1b8fb4',
          cancelButtonColor: '#d33',
          confirmButtonText: 'Yes, delete it!',
        }).then(async (result) => {
          if (result) {
            await this.$axios.delete(
              `https://zany-rose-alligator-yoke.cyclic.app/todo/${id}`
            )

            // Delete Object From Array On Way
            const item = this.allTodoLists.filter((el) => el.id !== id)
            this.allTodoLists = item

            // Delete Object From Array On Other Way
            // const index = this.allTodoLists.map((item) => item.id).indexOf(id) // find index
            // this.allTodoLists.splice(index, 1)
            this.$swal('Deleted!', 'Your List has been deleted.', 'success')
          }
        })
      } catch (err) {
        console.log(err)
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
      background: rgba(14, 107, 137, 0.6);
      cursor: unset;
    }
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
    .custom-style {
      display: flex;
      justify-content: end;
    }
    .custom-style {
      background: none;
      border: none;
      cursor: pointer;
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
