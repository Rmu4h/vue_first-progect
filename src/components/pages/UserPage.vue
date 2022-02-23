<template>
  <div >
    <h1>Page with post</h1>
    <my-input
      v-model="searchQuery"
      placeholder="Search..."
    />
    <div class="app__buttons">
      <my-button
        @click="showDialog"
      >
        Create post
      </my-button>
      <my-select 
        v-model="selectedSort"
        :options="sortOptions"
      />
    </div>

    <my-dialog v-model:show="dialogVisible">
      <post-form
        @create="createPost"
      />
    </my-dialog>
    <post-list 
      :posts="sortedAndSearchPosts"
      @remove="removePost"
      v-if="isPostsLoading !== true"
    />
    <div v-else>Loading...</div>
    <div ref="observer" class="observer">

    </div>
    <!-- <div class="page__wrapper">
      <div 
        v-for="pageNumber in totalPage" 
        :key="pageNumber"
        class="page"
        :class="{
          'current-page': page === pageNumber,
        }"
        @click="changePage(pageNumber)"
      >
        {{ pageNumber }}
      </div>
    </div> -->
  </div>
</template>

<script>
import PostForm from './components/PostForm.vue';
import PostList from "./components/PostList";
import MyButton from './components/UI/MyButton.vue';
import MySelect from './components/UI/MySelect.vue';
import axios from 'axios';
import MyInput from './components/UI/MyInput.vue';

  export default {
    components: {
      PostList, 
      PostForm,
      MyButton, 
      MySelect,
      MyInput,
    },
    data() {
      return {
        posts: [],
        dialogVisible: false,
        isPostsLoading: false,
        selectedSort: '',
        searchQuery: '',
        page: 1,
        limit: 10,
        totalPage: 0,
        sortOptions: [
          {value: 'title', name: 'by name'},
          {value: 'body', name: 'by description'}
        ]
      }
    },
    methods: {
      createPost(post) {
        this.posts.push(post);
        this.dialogVisible = false;
      },

      removePost(post) {
        this.posts = this.posts.filter(el => el.id !== post.id)
      },

      showDialog() {
        this.dialogVisible = true;
      },

      // changePage(pageNumber) {
      //   this.page = pageNumber;
      //   this.fetchPosts()
      // },

      async fetchPosts() {
        try {
          this.isPostsLoading = true;
            const response = await axios.get('https://jsonplaceholder.typicode.com/posts?_limit=10', {
              params: {
                _page: this.page,
                _limit: this.limit
              }
            });
            this.totalPage = Math.ceil(response.headers['x-total-count'] / this.limit)
            this.posts = response.data;
   
        } catch(e) {
          alert('Error')
        } finally {
            this.isPostsLoading = false;
        }
      },

      async loadMorePosts() {
        try {
          this.page += 1;
            const response = await axios.get('https://jsonplaceholder.typicode.com/posts?_limit=10', {
              params: {
                _page: this.page,
                _limit: this.limit
              }
            });
            this.totalPage = Math.ceil(response.headers['x-total-count'] / this.limit)
            this.posts = [...this.posts, ...response.data];
   
        } catch(e) {
          alert('Error')
        }
      },
    },

    mounted() {
      this.fetchPosts();
      console.log(this.$refs.observer);
      let options = {
        rootMargin: '0px',
        threshold: 1.0
      }

      const callback = (entries) => {
        if(entries[0].isIntersecting && this.page < this.totalPage) {
          this.loadMorePosts();
        }
      }

      let observer = new IntersectionObserver(callback, options);
      observer.observe(this.$refs.observer);

    },
    computed: {
      sortedPosts() {
        return  [...this.posts].sort((first, next) => {
          return first[this.selectedSort]?.localeCompare(next[this.selectedSort])
        })
      },
      sortedAndSearchPosts() {
        return this.sortedPosts.filter(post => post.title.toLowerCase().includes(this.searchQuery.toLowerCase()))
      }
    },
    watch: {
      // page() {
      //   this.fetchPosts()
      // }
    }
  }
</script>

<style>


.app__buttons {
  display: flex;
  justify-content: space-between;
}

.page__wrapper {
  display: flex;
  margin-top: 15px;

}
.page {
  border: 1px solid black;
  padding: 10px;
}

.current-page {
  border: 2px solid teal;
}

.observer {
  height: 30px;
  background: grey;
}

</style>
