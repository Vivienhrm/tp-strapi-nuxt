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
@import url('https://fonts.googleapis.com/css2?family=Outfit:wght@400;500;700&display=swap');

.blog-container {
  font-family: 'Outfit', sans-serif;
  max-width: 1200px;
  margin: 0 auto;
  padding: 3rem 1rem;
  background-color: #f8fafc;
  min-height: 100vh;
  color: #0f172a;
}

.blog-header {
  text-align: center;
  margin-bottom: 4rem;
}

.blog-header h1 {
  font-size: 3.5rem;
  font-weight: 700;
  color: #1e293b;
  margin-bottom: 1rem;
  letter-spacing: -1px;
}

.blog-header p {
  color: #64748b;
  font-size: 1.25rem;
}

.articles-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(320px, 1fr));
  gap: 2.5rem;
}

.article-card {
  background: white;
  border-radius: 16px;
  overflow: hidden;
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.05), 0 2px 4px -1px rgba(0, 0, 0, 0.03);
  transition: all 0.3s ease;
  display: flex;
  flex-direction: column;
  border: 1px solid #f1f5f9;
}

.article-card:hover {
  transform: translateY(-8px);
  box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.1), 0 10px 10px -5px rgba(0, 0, 0, 0.04);
}

.cover-wrapper {
  width: 100%;
  height: 220px;
  background-color: #e2e8f0;
  display: flex;
  align-items: center;
  justify-content: center;
}

.article-cover {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.no-cover {
  color: #94a3b8;
  font-weight: 500;
}

.article-content {
  padding: 2rem;
  flex-grow: 1;
  display: flex;
  flex-direction: column;
}

.article-content h2 {
  font-size: 1.5rem;
  font-weight: 700;
  color: #0f172a;
  margin-top: 0;
  margin-bottom: 1.5rem;
  line-height: 1.3;
}

.article-meta {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  margin-bottom: 1.5rem;
  font-size: 0.9rem;
  flex-wrap: wrap;
  gap: 1rem;
}

.author {
  color: #334155;
  font-weight: 500;
  background-color: #f1f5f9;
  padding: 0.35rem 0.75rem;
  border-radius: 8px;
}

.categories {
  display: flex;
  gap: 0.5rem;
  flex-wrap: wrap;
}

.category {
  background-color: #eff6ff;
  color: #3b82f6;
  padding: 0.35rem 0.75rem;
  border-radius: 8px;
  font-weight: 600;
  border: 1px solid #bfdbfe;
}

.article-text {
  color: #475569;
  line-height: 1.7;
  font-size: 1rem;
  margin-top: auto;
}

.loading {
  text-align: center;
  padding: 5rem;
  font-size: 1.5rem;
  color: #64748b;
  animation: pulse 2s infinite;
}

.error {
  text-align: center;
  padding: 3rem;
  background-color: #fee2e2;
  color: #b91c1c;
  border-radius: 12px;
  max-width: 600px;
  margin: 0 auto;
  font-weight: 500;
}

@keyframes pulse {
  0% { opacity: 1; }
  50% { opacity: 0.5; }
  100% { opacity: 1; }
}
</style>