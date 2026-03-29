<template>
  <div class="blog-container">
    <header class="blog-header">
      <h1>Mon Blog Headless 🚀</h1>
      <p>Bienvenue sur notre blog alimenté par Strapi V5 et Nuxt 3 !</p>
    </header>

    <main v-if="status === 'pending'" class="loading">
      Chargement des articles mystères...
    </main>
    <main v-else-if="error" class="error">
      Erreur lors du chargement (le serveur Strapi est-il allumé sur le port 1338 ?) : {{ error.message }}
    </main>
    
    <main v-else class="articles-grid">
      <article v-for="article in articles" :key="article.id" class="article-card">
        <div class="cover-wrapper">
          <img v-if="article.cover" :src="getStrapiMedia(article.cover.url)" :alt="article.title" class="article-cover" />
          <div v-else class="no-cover">Pas d'image</div>
        </div>
        
        <div class="article-content">
          <h2>{{ article.title }}</h2>
          
          <div class="article-meta">
            <span class="author" v-if="article.author">
              👤 {{ article.author.name }}
            </span>
            
            <div class="categories" v-if="article.categories && article.categories.length > 0">
              <span v-for="cat in article.categories" :key="cat.id" class="category">
                {{ cat.name }}
              </span>
            </div>
          </div>
          
          <div class="article-text">
            {{ formatContent(article.content) }}
          </div>
        </div>
      </article>
    </main>
  </div>
</template>

<script setup>
const STRAPI_URL = 'http://localhost:1338';

// fetch cote serveur
const { data: strapiResponse, error, status } = await useFetch(`${STRAPI_URL}/api/articles?populate=*`);

const articles = computed(() => {
  if (!strapiResponse.value) return [];
  return strapiResponse.value.data || [];
});

const getStrapiMedia = (url) => {
  if (url.startsWith('http')) return url;
  return `${STRAPI_URL}${url}`;
};

const formatContent = (content) => {
  if (!content) return '';
  if (typeof content === 'string') return content;
  if (Array.isArray(content)) {
    return content
      .map(block => block.children?.map(c => c.text).join(' '))
      .join('\n');
  }
  return '';
};
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap');

.blog-container {
  font-family: 'Inter', sans-serif;
  max-width: 1000px;
  margin: 0 auto;
  padding: 2rem 1rem;
  color: #333;
}

.blog-header {
  text-align: center;
  margin-bottom: 3rem;
}

.articles-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
  gap: 2rem;
}

.article-card {
  border: 1px solid #eee;
  border-radius: 12px;
  overflow: hidden;
  display: flex;
  flex-direction: column;
  transition: transform 0.2s;
}

.article-card:hover {
  transform: translateY(-5px);
  border-color: #3b82f6;
}

.article-cover {
  width: 100%;
  height: 200px;
  object-fit: cover;
  background: #eee;
}

.article-content {
  padding: 1.5rem;
}

.article-meta {
  font-size: 0.85rem;
  margin-bottom: 1rem;
  display: flex;
  gap: 10px;
  flex-wrap: wrap;
}

.author {
  font-weight: bold;
  color: #555;
}

.category {
  color: #3b82f6;
  background: #eff6ff;
  padding: 2px 8px;
  border-radius: 4px;
}

.article-text {
  color: #666;
  line-height: 1.5;
  display: -webkit-box;
  -webkit-line-clamp: 3;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

.loading, .error {
  text-align: center;
  padding: 3rem;
  font-weight: bold;
}

.error { color: red; }
</style>