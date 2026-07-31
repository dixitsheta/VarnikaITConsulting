---
title: "Search"
description: "Search Varnika IT Consulting's website for SAP analytics content, blog posts, and services."
---

<div id="search-container">
  <div class="search-wrapper">
    <span class="search-icon">🔍</span>
    <input type="text" id="search-input" placeholder="Search..." />
    <button id="clear-btn" class="clear-btn" style="display: none;">Clear</button>
  </div>
  <div id="search-results"></div>
</div>

<style>
#search-container {
  max-width: 1200px;
  margin: 60px auto;
  padding: 0 20px;
}

/* Search wrapper */
.search-wrapper {
  position: relative;
  margin-bottom: 2rem;
}

/* Search icon */
.search-icon {
  position: absolute;
  left: 1rem;
  top: 50%;
  transform: translateY(-50%);
  pointer-events: none;
  z-index: 2;
  font-size: 1rem;
}

/* Search input */
#search-input {
  width: 100%;
  border: 2px solid var(--border-color);
  border-radius: 8px;
  padding: 14px 5rem 14px 3rem;
  font-size: 16px;
  font-family: 'Inter', sans-serif;
  background: var(--bg-secondary);
  color: var(--text-primary);
  box-sizing: border-box;
}

#search-input:focus {
  border-color: var(--color-gold);
  outline: none;
  box-shadow: 0 0 0 3px rgba(244, 196, 48, 0.2);
}

/* Clear button */
.clear-btn {
  position: absolute;
  right: 1rem;
  top: 50%;
  transform: translateY(-50%);
  background: var(--bg-primary);
  color: var(--text-secondary);
  border: 1px solid var(--border-color);
  border-radius: 4px;
  padding: 0.35rem 0.75rem;
  font-size: 0.8rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.2s ease;
  z-index: 2;
}

.clear-btn:hover {
  background: var(--color-gold);
  color: var(--color-heading);
  border-color: var(--color-gold);
}

/* Results */
#search-results {
  margin-top: 2rem;
}

.search-result {
  border: 1px solid var(--border-color);
  border-radius: 8px;
  padding: 20px;
  margin-bottom: 16px;
  background: var(--bg-primary);
  transition: all 0.2s ease;
}

.search-result:hover {
  border-color: var(--color-gold);
  box-shadow: var(--shadow-md);
  transform: translateY(-2px);
}

.search-result h3 {
  color: var(--color-heading);
  font-size: 1.5rem;
  font-weight: 600;
  margin: 0 0 8px 0;
}

.search-result h3 a {
  color: var(--color-heading);
  text-decoration: none;
}

.search-result h3 a:hover {
  color: var(--color-gold);
}

.search-result p {
  color: var(--text-secondary);
  line-height: 1.6;
  margin: 0 0 8px 0;
}

.search-result .result-url {
  color: var(--text-secondary);
  font-size: 0.9rem;
  text-decoration: none;
}

.search-message {
  color: var(--text-secondary);
  font-size: 1rem;
  padding: 20px;
  text-align: center;
}

.loading {
  color: var(--color-gold);
  text-align: center;
  padding: 20px;
}
</style>

<script>
  let pagefind;
  
  async function initPagefind() {
    if (!pagefind) {
      pagefind = await import('/pagefind/pagefind.js');
    }
    return pagefind;
  }
  
  async function performSearch(query) {
    if (!query.trim()) {
      document.getElementById('search-results').innerHTML = '';
      return;
    }
    
    const resultsContainer = document.getElementById('search-results');
    resultsContainer.innerHTML = '<div class="loading">Searching...</div>';
    
    try {
      const pf = await initPagefind();
      const search = await pf.search(query);
      
      if (search.results.length === 0) {
        resultsContainer.innerHTML = '<div class="search-message">No results found for "' + query + '"</div>';
        return;
      }
      
      resultsContainer.innerHTML = '<div class="search-message">' + search.results.length + ' results for "' + query + '"</div>';
      
      for (const result of search.results.slice(0, 10)) {
        const data = await result.data();
        
        const resultDiv = document.createElement('div');
        resultDiv.className = 'search-result';
        
        resultDiv.innerHTML = `
          <h3><a href="${data.url}">${data.meta.title || 'Untitled'}</a></h3>
          <p>${data.excerpt}</p>
          <a href="${data.url}" class="result-url">${data.url}</a>
        `;
        
        resultsContainer.appendChild(resultDiv);
      }
    } catch (error) {
      console.error('Search error:', error);
      resultsContainer.innerHTML = '<div class="search-message">An error occurred while searching.</div>';
    }
  }
  
  window.addEventListener('DOMContentLoaded', function() {
    const input = document.getElementById('search-input');
    const clearBtn = document.getElementById('clear-btn');
    const resultsContainer = document.getElementById('search-results');
    
    let searchTimeout;
    
    // Handle input changes
    input.addEventListener('input', function() {
      clearTimeout(searchTimeout);
      
      if (input.value.trim()) {
        clearBtn.style.display = 'block';
        searchTimeout = setTimeout(() => performSearch(input.value), 300);
      } else {
        clearBtn.style.display = 'none';
        resultsContainer.innerHTML = '';
      }
    });
    
    // Handle clear button
    clearBtn.addEventListener('click', function() {
      input.value = '';
      clearBtn.style.display = 'none';
      resultsContainer.innerHTML = '';
      input.focus();
    });
    
    // Handle URL query parameter
    const urlParams = new URLSearchParams(window.location.search);
    const query = urlParams.get('q');
    if (query) {
      input.value = decodeURIComponent(query);
      clearBtn.style.display = 'block';
      performSearch(input.value);
    }
  });
</script>
