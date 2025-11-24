---
title: "Search"
description: "Search Varnika IT Consulting's website for SAP analytics content, blog posts, and services."
---

<div id="search-container">
  <div id="search"></div>
  <div id="search-results"></div>
</div>

<style>
#search-container {
  max-width: 1200px;
  margin: 60px auto;
  padding: 0 20px;
}

/* Override Pagefind default styles to match our golden theme */
.pagefind-ui__search-input {
  border: 2px solid var(--color-navy) !important;
  border-radius: 8px !important;
  padding: 12px 20px !important;
  font-size: 16px !important;
  font-family: 'Inter', sans-serif !important;
}

.pagefind-ui__search-input:focus {
  border-color: var(--color-gold) !important;
  outline: none !important;
  box-shadow: 0 0 0 3px rgba(244, 196, 48, 0.2) !important;
}

.pagefind-ui__result {
  border: 1px solid #e0e0e0 !important;
  border-radius: 8px !important;
  padding: 20px !important;
  margin-bottom: 16px !important;
  background: white !important;
  transition: all 0.2s ease !important;
}

.pagefind-ui__result:hover {
  border-color: var(--color-gold) !important;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1) !important;
  transform: translateY(-2px) !important;
}

.pagefind-ui__result-title {
  color: var(--color-navy) !important;
  font-size: 1.5rem !important;
  font-weight: 600 !important;
  margin-bottom: 8px !important;
}

.pagefind-ui__result-title:hover {
  color: var(--color-gold) !important;
}

.pagefind-ui__result-excerpt {
  color: var(--color-text-gray) !important;
  line-height: 1.6 !important;
  margin-bottom: 8px !important;
}

.pagefind-ui__result-link {
  color: var(--color-gold) !important;
  text-decoration: none !important;
  font-weight: 500 !important;
}

.pagefind-ui__message {
  color: var(--color-text-gray) !important;
  font-size: 1rem !important;
  padding: 20px !important;
  text-align: center !important;
}

.pagefind-ui__button {
  background: var(--color-gold) !important;
  color: var(--color-navy) !important;
  border: none !important;
  padding: 10px 20px !important;
  border-radius: 6px !important;
  font-weight: 600 !important;
  cursor: pointer !important;
  transition: all 0.2s ease !important;
}

.pagefind-ui__button:hover {
  background: var(--color-gold-dark) !important;
  transform: translateY(-2px) !important;
}

/* Search header */
#search-container h1 {
  color: var(--color-navy);
  font-size: 2.5rem;
  margin-bottom: 20px;
  text-align: center;
}

/* Loading state */
.pagefind-ui__loading {
  color: var(--color-gold) !important;
}
</style>

<script>
  // Initialize Pagefind when page loads
  window.addEventListener('DOMContentLoaded', (event) => {
    new PagefindUI({ 
      element: "#search",
      showSubResults: true,
      showImages: false,
      excerptLength: 30,
      resetStyles: false
    });
  });
</script>
