<template>
  <div>
    <form @submit.prevent="save" class="form-container">
      <input type="text" v-model="form.title" placeholder="Title" class="form-input" /><br />
      <textarea v-model="form.content" placeholder="Content" class="form-input"></textarea><br />
      <button type="submit" class="submit-button">{{ form.id ? 'Update' : 'Save' }}</button>
    </form>
    <ul>
      <li v-for="article in articles" :key="article.id">
        <div class="article">
          <h3>{{ article.title }}</h3>
          <p>{{ article.content }}</p>
          <button class="edit" @click="edit(article)">Edit</button>
          <button class="delete" @click="remove(article.id)">Delete</button>
        </div>
      </li>
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
    save() {
      if (this.form.id) {
        // Update existing article
        const index = this.articles.findIndex(article => article.id === this.form.id);
        if (index !== -1) {
          this.articles.splice(index, 1, { ...this.form });
        }
      } else {
        // Add new article
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
    load() {
      // Here we simulate loading data, replace with actual data fetching
      this.articles = [
        { id: '1', title: 'judul', content: 'Ini Content' },
        { id: '2', title: 'judul 2', content: 'Ini Content 2' },
        { id: '3', title: 'judul 3', content: 'Ini Content 3' }
      ];
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
    this.load(); // Load articles when component is mounted
  }
};
</script>

<style>
/* Gaya umum untuk input dan textarea */
.form-input {
  width: calc(100% - 20px);
  padding: 10px;
  margin-bottom: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
  box-shadow: inset 0 1px 3px rgba(0, 0, 0, 0.1);
}

/* Gaya umum untuk tombol */
button {
  padding: 10px 15px;
  margin-right: 5px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s ease, transform 0.2s ease, box-shadow 0.3s ease;
}

button:hover {
  transform: translateY(-2px);
}

button:active {
  transform: translateY(0);
}

/* Tombol submit */
.submit-button {
  background: linear-gradient(45deg, #6a11cb, #2575fc);
  color: white;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.submit-button:hover {
  box-shadow: 0 6px 8px rgba(0, 0, 0, 0.2);
}

.submit-button:active {
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
}

/* Tombol edit */
button.edit {
  background: linear-gradient(45deg, #00c6ff, #0072ff);
  color: white;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

button.edit:hover {
  box-shadow: 0 6px 8px rgba(0, 0, 0, 0.2);
}

button.edit:active {
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
}

/* Tombol delete */
button.delete {
  background: linear-gradient(45deg, #f85032, #e73827);
  color: white;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

button.delete:hover {
  box-shadow: 0 6px 8px rgba(0, 0, 0, 0.2);
}

button.delete:active {
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
}

/* Tombol load */
.load-button {
  background: linear-gradient(45deg, #36d1dc, #5b86e5);
  color: white;
  padding: 10px 20px;
  border-radius: 5px;
  border: none;
  cursor: pointer;
  transition: background-color 0.3s ease, transform 0.2s ease, box-shadow 0.3s ease;
}

.load-button:hover {
  box-shadow: 0 6px 8px rgba(0, 0, 0, 0.2);
}

.load-button:active {
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
  transform: translateY(0);
}

/* Gaya tambahan untuk kontainer artikel */
.article {
  background-color: #ffffff;
  padding: 15px;
  margin-bottom: 10px;
  border-radius: 5px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.article h3 {
  margin-top: 0;
}

.article p {
  margin-bottom: 0;
}

/* Gaya untuk kontainer form */
.form-container {
  background: linear-gradient(45deg, #ece9e6, #ffffff);
  padding: 20px;
  border-radius: 5px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  margin-bottom: 20px;
}
</style>
