<template>
  <div>
    <form @submit.prevent="save" class="form-container">
      <input type="text" v-model="form.title" placeholder="Title" class="form-input" /><br />
      <textarea v-model="form.content" placeholder="Content" class="form-input"></textarea><br />
      <button type="submit" class="submit-button">{{ form.id ? 'Update' : 'Save' }}</button>
    </form>
    <ul class="articles-list">
      <transition-group name="list" tag="ul">
        <li v-for="article in articles" :key="article.id" class="article-item">
          <div class="article">
            <h3>{{ article.title }}</h3>
            <p>{{ article.content }}</p>
            <div class="buttons">
              <button class="edit" @click="edit(article)">Edit</button>
              <button class="delete" @click="remove(article.id)">Delete</button>
            </div>
          </div>
        </li>
      </transition-group>
    </ul>
    <button @click="load" class="load-button">Load</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      articles: [],
      form: {
        id: null,
        title: '',
        content: ''
      }
    };
  },
  methods: {
    async save() {
      if (this.form.id) {
        const index = this.articles.findIndex(article => article.id === this.form.id);
        if (index !== -1) {
          this.articles.splice(index, 1, { ...this.form });
        }
      } else {
        const newArticle = { ...this.form, id: Date.now().toString() };
        this.articles.push(newArticle);
      }
      this.resetForm();
    },
    edit(article) {
      this.form = { ...article };
    },
    remove(id) {
      const index = this.articles.findIndex(article => article.id === id);
      if (index !== -1) {
        this.articles.splice(index, 1);
      }
    },
    async load() {
      try {
        const response = await fetch('/db.json');
        if (!response.ok) {
          throw new Error(`HTTP error! status: ${response.status}`);
        }
        const data = await response.json();
        this.articles = data.articles;
      } catch (error) {
        console.error('Error loading articles:', error);
      }
    },
    resetForm() {
      this.form = {
        id: null,
        title: '',
        content: ''
      };
    }
  },
  mounted() {
    this.load();
  }
};
</script>

<style>
body {
  font-family: 'Arial', sans-serif;
  background: linear-gradient(to right, #e3f2fd, #e1bee7);
  margin: 0;
  padding: 0;
}

.form-container {
  background: linear-gradient(to right, #ffffff, #f8bbd0);
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  margin-bottom: 20px;
  max-width: 600px;
  margin: 20px auto;
  transition: all 0.3s ease-in-out;
  animation: fadeIn 0.6s ease-in-out;
}

.form-input {
  width: calc(100% - 20px);
  padding: 10px;
  margin-bottom: 15px;
  border: 1px solid #ddd;
  border-radius: 5px;
  font-size: 16px;
  box-shadow: inset 0 2px 4px rgba(0, 0, 0, 0.1);
  background: #f9f9fb;
  transition: all 0.3s ease-in-out;
}

.form-input:focus {
  border-color: #007bff;
  box-shadow: 0 0 8px rgba(0, 123, 255, 0.2);
}

.submit-button {
  background: linear-gradient(to right, #ff6f00, #ff8f00);
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-size: 16px;
  transition: background-color 0.3s, transform 0.2s, box-shadow 0.3s;
}

.submit-button:hover {
  background: linear-gradient(to right, #ff8f00, #ff6f00);
  transform: translateY(-2px);
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
}

.submit-button:active {
  background: linear-gradient(to right, #ff6f00, #ff8f00);
  transform: translateY(0);
}

.articles-list {
  max-width: 600px;
  margin: 20px auto;
  padding: 0;
  list-style: none;
}

.article-item {
  margin-bottom: 15px;
  transition: all 0.3s ease-in-out;
  animation: fadeIn 0.6s ease-in-out;
}

.article {
  background: linear-gradient(to right, #ffffff, #e0f7fa);
  padding: 15px;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  transform: translateY(0);
  transition: all 0.3s ease-in-out;
}

.article:hover {
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
  transform: translateY(-2px);
}

.article h3 {
  margin-top: 0;
  font-size: 18px;
  color: #333;
}

.article p {
  margin: 10px 0;
  font-size: 14px;
  color: #666;
}

.buttons {
  display: flex;
  gap: 10px;
}

button.edit {
  background: linear-gradient(to right, #4caf50, #66bb6a);
  color: white;
  padding: 8px 12px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s, transform 0.2s, box-shadow 0.3s;
}

button.edit:hover {
  background: linear-gradient(to right, #66bb6a, #4caf50);
  transform: translateY(-2px);
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
}

button.edit:active {
  background: linear-gradient(to right, #4caf50, #66bb6a);
  transform: translateY(0);
}

button.delete {
  background: linear-gradient(to right, #e53935, #f44336);
  color: white;
  padding: 8px 12px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s, transform 0.2s, box-shadow 0.3s;
}

button.delete:hover {
  background: linear-gradient(to right, #f44336, #e53935);
  transform: translateY(-2px);
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
}

button.delete:active {
  background: linear-gradient(to right, #e53935, #f44336);
  transform: translateY(0);
}

.load-button {
  background: linear-gradient(to right, #2196f3, #42a5f5);
  color: white;
  padding: 10px 20px;
  border-radius: 5px;
  border: none;
  cursor: pointer;
  transition: background-color 0.3s, transform 0.2s, box-shadow 0.3s;
  display: block;
  margin: 20px auto;
}

.load-button:hover {
  background: linear-gradient(to right, #42a5f5, #2196f3);
  transform: translateY(-2px);
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
}

.load-button:active {
  background: linear-gradient(to right, #2196f3, #42a5f5);
  transform: translateY(0);
}

.list-enter-active, .list-leave-active {
  transition: all 0.5s ease;
}

.list-enter, .list-leave-to {
  opacity: 0;
  transform: translateY(20px);
}

@keyframes fadeIn {
  0% {
    opacity: 0;
    transform: translateY(20px);
  }
  100% {
    opacity: 1;
    transform: translateY(0);
  }
}
</style>
