<script>
  const books = [
    {
      title: "The Great Gatsby",
      author: "F. Scott Fitzgerald",
      image: "https://covers.openlibrary.org/b/id/7222246-L.jpg",
    },
    {
      title: "To Kill a Mockingbird",
      author: "Harper Lee",
      image: "https://covers.openlibrary.org/b/id/8228691-L.jpg",
    },
    {
      title: "1984",
      author: "George Orwell",
      image: "https://covers.openlibrary.org/b/id/7222246-L.jpg",
    },
    {
      title: "Pride and Prejudice",
      anthor: "Jane Austen",
      image: "https://covers.openlibrary.org/b/id/8228691-L.jpg",
    },
    {
      title: "The Great Gatsby",
      author: "F. Scott Fitzgerald",
      image: "https://covers.openlibrary.org/b/id/7222246-L.jpg",
    },
    {
      title: "To Kill a Mockingbird",
      author: "Harper Lee",
      image: "https://covers.openlibrary.org/b/id/8228691-L.jpg",
    },
    {
      title: "1984",
      author: "George Orwell",
      image: "https://covers.openlibrary.org/b/id/7222246-L.jpg",
    },
    {
      title: "Pride and Prejudice",
      anthor: "Jane Austen",
      image: "https://covers.openlibrary.org/b/id/8228691-L.jpg",
    },
    {
      title: "The Great Gatsby",
      author: "F. Scott Fitzgerald",
      image: "https://covers.openlibrary.org/b/id/7222246-L.jpg",
    },
    {
      title: "To Kill a Mockingbird",
      author: "Harper Lee",
      image: "https://covers.openlibrary.org/b/id/8228691-L.jpg",
    },
    {
      title: "1984",
      author: "George Orwell",
      image: "https://covers.openlibrary.org/b/id/7222246-L.jpg",
    },
    {
      title: "Pride and Prejudice",
      anthor: "Jane Austen",
      image: "https://covers.openlibrary.org/b/id/8228691-L.jpg",
    },
  ];
</script>

<div>
  <div class="library-header">
    <h1>Welcome to the Library</h1>
    <p>Explore our collection of books</p>
  </div>
  <div class="library-search">
    <input type="text" placeholder="Search for books..." />
    <button>Search</button>
  </div>
  <div class="library-controls">
    <div class="library-filter">
      <label for="genre">Filter by Genre:</label>
      <select id="genre">
        <option value="all">All</option>
        <option value="fiction">Fiction</option>
        <option value="non-fiction">Non-Fiction</option>
        <option value="mystery">Mystery</option>
        <option value="fantasy">Fantasy</option>
      </select>
      <button>Apply</button>
    </div>
    <div class="library-sort">
      <label for="sort">Sort by:</label>
      <select id="sort">
        <option value="title">Title</option>
        <option value="author">Author</option>
        <option value="date">Publication Date</option>
      </select>
      <button>Sort</button>
    </div>
  </div>
  <div class="library-actions">
    <button>Add New Book</button>
    <button>Delete Selected Books</button>
    <button>Export to CSV</button>
    <button>Import from CSV</button>
    <button>Print</button>
    <button>Share</button>
  </div>

  <div class="library-container">
    <h2>Library Books</h2>
    <div class="book-list">
      {#each books as book}
        <div class="book-card">
          <img class="book-cover" src={book.image} alt={book.title} />
          <div class="book-info">
            <div class="book-title">{book.title}</div>
            <div class="book-author">by {book.author}</div>
          </div>
        </div>
      {/each}
    </div>
  </div>
</div>

<style>
  .library-container {
    padding: 2rem;
    background: #f9f9f9;
    border-radius: 1rem;
    max-width: 1800px;
    margin: 2rem auto;
  }

  h2 {
    text-align: center;
    font-size: 2rem;
    margin-bottom: 1.5rem;
    color: #333;
  }

  .book-list {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    gap: 1.5rem;
  }

  .book-card {
    background: white;
    border-radius: 0.5rem;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.05);
    overflow: hidden;
    transition: transform 0.2s ease;
  }

  .book-card:hover {
    transform: scale(1.02);
  }

  .book-cover {
    width: 100%;
    height: 250px;
    object-fit: cover;
  }

  .book-info {
    padding: 1rem;
  }

  .book-title {
    font-size: 1.1rem;
    font-weight: bold;
    margin-bottom: 0.5rem;
    color: #222;
  }

  .book-author {
    color: #666;
    font-size: 0.9rem;
  }
  .library-controls {
    display: flex;
    justify-content: center;
    align-items: center;
    margin-bottom: 1.5rem;
    gap: 1rem;
  }
  .library-header {
    text-align: center;
    margin-bottom: 2rem;
  }
  .library-header h1 {
    font-size: 2.5rem;
    color: #333;
    text-align: center;
  }
  .library-header p {
    font-size: 1.2rem;
    color: #666;
  }
  .library-search {
    display: flex;
    justify-content: center;
    margin-bottom: 1.5rem;
  }
  .library-search input {
    padding: 0.5rem;
    border: 1px solid #ccc;
    border-radius: 0.5rem;
    width: 300px;
    margin-right: 0.5rem;
  }
  .library-search button {
    padding: 0.5rem 1rem;
    background-color: #007bff;
    color: white;
    border: none;
    border-radius: 0.5rem;
    cursor: pointer;
  }
  .library-search button:hover {
    background-color: #0056b3;
  }
  .library-filter,
  .library-sort {
    display: flex;
    justify-content: center;
    margin-bottom: 1.5rem;
  }
  .library-filter label,
  .library-sort label {
    margin-right: 0.5rem;
    font-size: 1rem;
    color: #333;
  }
  .library-filter select,
  .library-sort select {
    padding: 0.5rem;
    border: 1px solid #ccc;
    border-radius: 0.5rem;
    margin-right: 0.5rem;
  }
  .library-filter button,
  .library-sort button {
    padding: 0.5rem 1rem;
    background-color: #007bff;
    color: white;
    border: none;
    border-radius: 0.5rem;
    cursor: pointer;
  }
  .library-filter button:hover,
  .library-sort button:hover {
    background-color: #0056b3;
  }
  .library-actions {
    display: flex;
    justify-content: center;
    margin-bottom: 1.5rem;
  }
  .library-actions button {
    padding: 0.5rem 1rem;
    background-color: #007bff;
    color: white;
    border: none;
    border-radius: 0.5rem;
    cursor: pointer;
    margin: 0 0.5rem;
  }
  .library-actions button:hover {
    background-color: #0056b3;
  }
  .library-actions button:disabled {
    background-color: #ccc;
    cursor: not-allowed;
  }
  .library-actions button:disabled:hover {
    background-color: #ccc;
  }
  .library-actions button:not(:disabled) {
    transition: background-color 0.2s ease;
  }
  .library-actions button:not(:disabled):hover {
    background-color: #0056b3;
  }
  .library-actions button:not(:disabled):active {
    background-color: #004494;
  }
  .library-actions button:not(:disabled):focus {
    outline: none;
    box-shadow: 0 0 0 2px rgba(0, 123, 255, 0.5);
  }
  .library-actions button:not(:disabled):focus-visible {
    outline: none;
    box-shadow: 0 0 0 2px rgba(0, 123, 255, 0.5);
  }
  .library-actions button:not(:disabled):focus-visible:active {
    background-color: #004494;
  }
  .library-actions button:not(:disabled):focus-visible:active:focus {
    outline: none;
    box-shadow: 0 0 0 2px rgba(0, 123, 255, 0.5);
  }
</style>
