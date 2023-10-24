<template>
  <div class="main">
    <div class="search-container">
      <div class="border search-input">
        <input type="text" v-model.trim="searchString" placeholder="@username" @input="getUsersProfile()" />
        <button v-show="searchString" @click="changeVisibility()">
          <IconCollapse :class="isVisible ? '' : 'collapsed'"/>
        </button>
      </div>
      <div v-show="isVisible" :class="{'border no-top-border': filteredList.length > 0}">
        <div v-for="user in filteredList" :key="user" class="user-item">
          @{{ user.username.toLowerCase() }}
          <span v-if="user.name" class="user-name">
            {{ user.name }}
          </span>
        </div>
      </div>
      <div v-show="filteredList.length === 0 && searchString">
        Нет результатов
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import IconCollapse from './icons/IconCollapse.vue';
export default {

  components: {
    IconCollapse
  },

  data() {
    return {
      posts: null,
      searchString: '',
      usersList: null,
      endpoint: 'https://jsonplaceholder.typicode.com/users',
      isVisible: false,
    }
  },

  created() {
    this.getUsersProfile()
  },

  methods: {
    async getUsersProfile() {
      await axios.get(this.endpoint + '?q=' + this.searchString)
        .then(response => {
          this.isVisible = true;
          this.usersList = response.data
        })
        .catch(error => {
          console.log(error)
        })
    },
    changeVisibility() {
      this.isVisible = !this.isVisible;
    }
  },

  computed: {
    filteredList() {
      return this.usersList && this.searchString.length ? this.usersList.filter(item => item.username.toLowerCase().startsWith(this.searchString.toLowerCase())) : [];
    }
  }
}
</script>
