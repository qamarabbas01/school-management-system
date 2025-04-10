<script>
    import { onMount } from 'svelte';
    
    // State management
    let currentUser = null;
    let isAuthenticated = false;
    let currentRole = null;
    let activeView = 'login';
    let searchQuery = '';
    let searchType = 'title';
    let books = [];
    let filteredBooks = [];
    let selectedBook = null;
    let users = [];
    let loans = [];
    let darkMode = false;
    
    // Mock data
    function generateMockData() {
      books = [
        { 
          id: 1, 
          title: 'To Kill a Mockingbird', 
          author: 'Harper Lee', 
          isbn: '9780061120084', 
          publishYear: 1960,
          category: 'Fiction',
          available: true,
          coverUrl: '/placeholder.svg?height=200&width=150',
          description: 'The unforgettable novel of a childhood in a sleepy Southern town and the crisis of conscience that rocked it.'
        },
        { 
          id: 2, 
          title: '1984', 
          author: 'George Orwell', 
          isbn: '9780451524935', 
          publishYear: 1949,
          category: 'Science Fiction',
          available: false,
          coverUrl: '/placeholder.svg?height=200&width=150',
          description: 'A dystopian novel set in Airstrip One, a province of the superstate Oceania in a world of perpetual war.'
        },
        { 
          id: 3, 
          title: 'The Great Gatsby', 
          author: 'F. Scott Fitzgerald', 
          isbn: '9780743273565', 
          publishYear: 1925,
          category: 'Fiction',
          available: true,
          coverUrl: '/placeholder.svg?height=200&width=150',
          description: 'A novel of the Jazz Age that follows mysterious millionaire Jay Gatsby and his obsession with Daisy Buchanan.'
        },
        { 
          id: 4, 
          title: 'Pride and Prejudice', 
          author: 'Jane Austen', 
          isbn: '9780141439518', 
          publishYear: 1813,
          category: 'Classic',
          available: true,
          coverUrl: '/placeholder.svg?height=200&width=150',
          description: 'The story follows the main character Elizabeth Bennet as she deals with issues of manners, upbringing, and marriage.'
        },
        { 
          id: 5, 
          title: 'The Hobbit', 
          author: 'J.R.R. Tolkien', 
          isbn: '9780547928227', 
          publishYear: 1937,
          category: 'Fantasy',
          available: false,
          coverUrl: '/placeholder.svg?height=200&width=150',
          description: 'The adventure of Bilbo Baggins, a hobbit who embarks on an unexpected journey.'
        }
      ];
      
      users = [
        { id: 1, name: 'John Smith', role: 'student', email: 'john@school.edu', borrowedBooks: [2] },
        { id: 2, name: 'Sarah Johnson', role: 'student', email: 'sarah@school.edu', borrowedBooks: [5] },
        { id: 3, name: 'Admin User', role: 'librarian', email: 'admin@school.edu', borrowedBooks: [] }
      ];
      
      loans = [
        { id: 1, bookId: 2, userId: 1, issueDate: '2023-05-10', dueDate: '2023-05-24', returned: false },
        { id: 2, bookId: 5, userId: 2, issueDate: '2023-05-15', dueDate: '2023-05-29', returned: false }
      ];
      
      filteredBooks = [...books];
    }
    
    // Authentication functions
    function login(email, password) {
      // In a real app, this would validate against a backend
      const user = users.find(u => u.email === email);
      if (user && password === 'password') { // Simplified for demo
        currentUser = user;
        currentRole = user.role;
        isAuthenticated = true;
        activeView = 'dashboard';
        return true;
      }
      return false;
    }
    
    function logout() {
      currentUser = null;
      currentRole = null;
      isAuthenticated = false;
      activeView = 'login';
    }
    
    // Book management functions
    function searchBooks() {
      if (!searchQuery.trim()) {
        filteredBooks = [...books];
        return;
      }
      
      const query = searchQuery.toLowerCase();
      filteredBooks = books.filter(book => {
        if (searchType === 'title') return book.title.toLowerCase().includes(query);
        if (searchType === 'author') return book.author.toLowerCase().includes(query);
        if (searchType === 'isbn') return book.isbn.includes(query);
        return false;
      });
    }
    
    function viewBookDetails(book) {
      selectedBook = book;
      activeView = 'bookDetails';
    }
    
    function addNewBook(bookData) {
      const newId = Math.max(...books.map(b => b.id)) + 1;
      const newBook = {
        id: newId,
        ...bookData,
        available: true,
        coverUrl: '/placeholder.svg?height=200&width=150'
      };
      books = [...books, newBook];
      filteredBooks = [...books];
      return newBook;
    }
    
    function borrowBook(bookId) {
      if (!currentUser) return false;
      
      const book = books.find(b => b.id === bookId);
      if (!book || !book.available) return false;
      
      // Update book availability
      books = books.map(b => {
        if (b.id === bookId) {
          return { ...b, available: false };
        }
        return b;
      });
      
      // Create loan record
      const today = new Date();
      const dueDate = new Date();
      dueDate.setDate(today.getDate() + 14); // 2 weeks loan period
      
      const newLoan = {
        id: loans.length + 1,
        bookId,
        userId: currentUser.id,
        issueDate: today.toISOString().split('T')[0],
        dueDate: dueDate.toISOString().split('T')[0],
        returned: false
      };
      
      loans = [...loans, newLoan];
      
      // Update user's borrowed books
      users = users.map(u => {
        if (u.id === currentUser.id) {
          return {
            ...u,
            borrowedBooks: [...u.borrowedBooks, bookId]
          };
        }
        return u;
      });
      
      if (currentUser.id === currentUser.id) {
        currentUser = users.find(u => u.id === currentUser.id);
      }
      
      filteredBooks = [...books];
      return true;
    }
    
    function returnBook(bookId) {
      // Update loan record
      loans = loans.map(loan => {
        if (loan.bookId === bookId && loan.userId === currentUser.id && !loan.returned) {
          return { ...loan, returned: true };
        }
        return loan;
      });
      
      // Update book availability
      books = books.map(b => {
        if (b.id === bookId) {
          return { ...b, available: true };
        }
        return b;
      });
      
      // Update user's borrowed books
      users = users.map(u => {
        if (u.id === currentUser.id) {
          return {
            ...u,
            borrowedBooks: u.borrowedBooks.filter(id => id !== bookId)
          };
        }
        return u;
      });
      
      if (currentUser.id === currentUser.id) {
        currentUser = users.find(u => u.id === currentUser.id);
      }
      
      filteredBooks = [...books];
      return true;
    }
    
    // User management functions (for librarian)
    function addUser(userData) {
      const newId = Math.max(...users.map(u => u.id)) + 1;
      const newUser = {
        id: newId,
        ...userData,
        borrowedBooks: []
      };
      users = [...users, newUser];
      return newUser;
    }
    
    // Toggle dark mode
    function toggleDarkMode() {
      darkMode = !darkMode;
    }
    
    // Initialize data on component mount
    onMount(() => {
      generateMockData();
    });
    
    // Form state
    let loginEmail = 'admin@school.edu'; // Pre-filled for demo
    let loginPassword = 'password'; // Pre-filled for demo
    let loginError = '';
    
    let newBookData = {
      title: '',
      author: '',
      isbn: '',
      publishYear: new Date().getFullYear(),
      category: 'Fiction',
      description: ''
    };
    
    let newUserData = {
      name: '',
      email: '',
      role: 'student'
    };
    
    // Handle form submissions
    function handleLogin() {
      if (!loginEmail || !loginPassword) {
        loginError = 'Please enter both email and password';
        return;
      }
      
      const success = login(loginEmail, loginPassword);
      if (!success) {
        loginError = 'Invalid credentials';
      }
    }
    
    function handleAddBook() {
      if (!newBookData.title || !newBookData.author || !newBookData.isbn) {
        return;
      }
      
      addNewBook(newBookData);
      newBookData = {
        title: '',
        author: '',
        isbn: '',
        publishYear: new Date().getFullYear(),
        category: 'Fiction',
        description: ''
      };
      activeView = 'manageBooks';
    }
    
    function handleAddUser() {
      if (!newUserData.name || !newUserData.email) {
        return;
      }
      
      addUser(newUserData);
      newUserData = {
        name: '',
        email: '',
        role: 'student'
      };
      activeView = 'manageUsers';
    }
  </script>
  
  <main class={darkMode ? 'dark-mode' : ''}>
    <!-- Header/Navigation -->
    <header>
      <div class="logo">
        <h1>School Library</h1>
      </div>
      
      {#if isAuthenticated}
        <nav>
          <ul>
            <li><button class={activeView === 'dashboard' ? 'active' : ''} on:click={() => activeView = 'dashboard'}>Dashboard</button></li>
            <li><button class={activeView === 'books' ? 'active' : ''} on:click={() => activeView = 'books'}>Books</button></li>
            
            {#if currentRole === 'librarian'}
              <li><button class={activeView === 'manageBooks' ? 'active' : ''} on:click={() => activeView = 'manageBooks'}>Manage Books</button></li>
              <li><button class={activeView === 'manageUsers' ? 'active' : ''} on:click={() => activeView = 'manageUsers'}>Manage Users</button></li>
              <li><button class={activeView === 'loans' ? 'active' : ''} on:click={() => activeView = 'loans'}>Loans</button></li>
            {/if}
            
            <li><button class={activeView === 'myBooks' ? 'active' : ''} on:click={() => activeView = 'myBooks'}>My Books</button></li>
          </ul>
        </nav>
        
        <div class="user-controls">
          <span class="user-name">{currentUser.name}</span>
          <span class="user-role">{currentRole}</span>
          <button class="theme-toggle" on:click={toggleDarkMode}>
            {darkMode ? '☀️' : '🌙'}
          </button>
          <button class="logout-btn" on:click={logout}>Logout</button>
        </div>
      {:else}
        <div class="theme-toggle-container">
          <button class="theme-toggle" on:click={toggleDarkMode}>
            {darkMode ? '☀️' : '🌙'}
          </button>
        </div>
      {/if}
    </header>
    
    <!-- Main Content Area -->
    <div class="content">
      <!-- Login View -->
      {#if activeView === 'login'}
        <div class="auth-container">
          <div class="auth-form">
            <h2>Login to Library System</h2>
            
            {#if loginError}
              <div class="error-message">{loginError}</div>
            {/if}
            
            <div class="form-group">
              <label for="email">Email</label>
              <input 
                type="email" 
                id="email" 
                bind:value={loginEmail} 
                placeholder="Enter your email"
              />
            </div>
            
            <div class="form-group">
              <label for="password">Password</label>
              <input 
                type="password" 
                id="password" 
                bind:value={loginPassword} 
                placeholder="Enter your password"
              />
            </div>
            
            <button class="btn-primary" on:click={handleLogin}>Login</button>
            
            <div class="demo-info">
              <p>Demo Accounts:</p>
              <p>Librarian: admin@school.edu / password</p>
              <p>Student: john@school.edu / password</p>
            </div>
          </div>
        </div>
      
      <!-- Dashboard View -->
      {:else if activeView === 'dashboard'}
        <div class="dashboard">
          <h2>Welcome, {currentUser.name}</h2>
          
          <div class="dashboard-stats">
            <div class="stat-card">
              <h3>Total Books</h3>
              <p class="stat-number">{books.length}</p>
            </div>
            
            <div class="stat-card">
              <h3>Available Books</h3>
              <p class="stat-number">{books.filter(b => b.available).length}</p>
            </div>
            
            {#if currentRole === 'librarian'}
              <div class="stat-card">
                <h3>Total Users</h3>
                <p class="stat-number">{users.length}</p>
              </div>
              
              <div class="stat-card">
                <h3>Active Loans</h3>
                <p class="stat-number">{loans.filter(l => !l.returned).length}</p>
              </div>
            {:else}
              <div class="stat-card">
                <h3>My Books</h3>
                <p class="stat-number">{currentUser.borrowedBooks.length}</p>
              </div>
            {/if}
          </div>
          
          <div class="quick-actions">
            <h3>Quick Actions</h3>
            <div class="action-buttons">
              <button class="btn-primary" on:click={() => activeView = 'books'}>Browse Books</button>
              
              {#if currentRole === 'librarian'}
                <button class="btn-primary" on:click={() => activeView = 'addBook'}>Add New Book</button>
                <button class="btn-primary" on:click={() => activeView = 'manageUsers'}>Manage Users</button>
              {/if}
              
              <button class="btn-primary" on:click={() => activeView = 'myBooks'}>My Borrowed Books</button>
            </div>
          </div>
        </div>
      
      <!-- Books View -->
      {:else if activeView === 'books'}
        <div class="books-view">
          <h2>Book Catalog</h2>
          
          <div class="search-container">
            <div class="search-controls">
              <select bind:value={searchType}>
                <option value="title">Title</option>
                <option value="author">Author</option>
                <option value="isbn">ISBN</option>
              </select>
              
              <input 
                type="text" 
                bind:value={searchQuery} 
                placeholder={`Search by ${searchType}...`}
                on:input={searchBooks}
              />
              
              <button class="btn-primary" on:click={searchBooks}>Search</button>
            </div>
          </div>
          
          <div class="books-grid">
            {#each filteredBooks as book (book.id)}
            <div class="book-card">
                <div class="book-cover">
                  <img src={book.coverUrl || "/placeholder.svg"} alt={`Cover of ${book.title}`} />
                  <div class="book-status" class:available={book.available}>
                    {book.available ? 'Available' : 'Borrowed'}
                  </div>
                </div>
                <div class="book-info">
                  <h3 class="book-title">{book.title}</h3>
                  <p class="book-author">by {book.author}</p>
                  <p class="book-category">{book.category}</p>
                </div>
              </div>
            {:else}
              <div class="no-results">No books found matching your search.</div>
            {/each}
          </div>
        </div>
      
      <!-- Book Details View -->
      {:else if activeView === 'bookDetails' && selectedBook}
        <div class="book-details">
          <button class="back-button" on:click={() => activeView = 'books'}>← Back to Books</button>
          
          <div class="book-details-content">
            <div class="book-cover-large">
              <img src={selectedBook.coverUrl || "/placeholder.svg"} alt={`Cover of ${selectedBook.title}`} />
            </div>
            
            <div class="book-info-detailed">
              <h2>{selectedBook.title}</h2>
              <p class="author">by {selectedBook.author}</p>
              
              <div class="book-metadata">
                <p><strong>ISBN:</strong> {selectedBook.isbn}</p>
                <p><strong>Published:</strong> {selectedBook.publishYear}</p>
                <p><strong>Category:</strong> {selectedBook.category}</p>
                <p class="availability" class:available={selectedBook.available}>
                  <strong>Status:</strong> {selectedBook.available ? 'Available' : 'Borrowed'}
                </p>
              </div>
              
              <div class="book-description">
                <h3>Description</h3>
                <p>{selectedBook.description}</p>
              </div>
              
              <div class="book-actions">
                {#if selectedBook.available}
                  <button 
                    class="btn-primary" 
                    on:click={() => borrowBook(selectedBook.id)}
                    disabled={currentRole === 'librarian'}
                  >
                    Borrow Book
                  </button>
                {:else if currentUser.borrowedBooks.includes(selectedBook.id)}
                  <button 
                    class="btn-secondary" 
                    on:click={() => returnBook(selectedBook.id)}
                  >
                    Return Book
                  </button>
                {/if}
                
                {#if currentRole === 'librarian'}
                  <button class="btn-secondary">Edit Book</button>
                  <button class="btn-danger">Remove Book</button>
                {/if}
              </div>
            </div>
          </div>
        </div>
      
      <!-- My Books View -->
      {:else if activeView === 'myBooks'}
        <div class="my-books">
          <h2>My Borrowed Books</h2>
          
          {#if currentUser.borrowedBooks.length === 0}
            <div class="no-results">You haven't borrowed any books yet.</div>
          {:else}
            <div class="loans-table">
              <table>
                <thead>
                  <tr>
                    <th>Book Title</th>
                    <th>Author</th>
                    <th>Borrowed Date</th>
                    <th>Due Date</th>
                    <th>Actions</th>
                  </tr>
                </thead>
                <tbody>
                  {#each loans.filter(l => l.userId === currentUser.id && !l.returned) as loan (loan.id)}
                    {@const book = books.find(b => b.id === loan.bookId)}
                    <tr>
                      <td>{book.title}</td>
                      <td>{book.author}</td>
                      <td>{loan.issueDate}</td>
                      <td class={new Date(loan.dueDate) < new Date() ? 'overdue' : ''}>{loan.dueDate}</td>
                      <td>
                        <button 
                          class="btn-small" 
                          on:click={() => returnBook(book.id)}
                        >
                          Return
                        </button>
                        <button 
                          class="btn-small btn-secondary" 
                          on:click={() => viewBookDetails(book)}
                        >
                          Details
                        </button>
                      </td>
                    </tr>
                  {/each}
                </tbody>
              </table>
            </div>
          {/if}
        </div>
      
      <!-- Librarian: Manage Books View -->
      {:else if activeView === 'manageBooks' && currentRole === 'librarian'}
        <div class="manage-books">
          <h2>Manage Books</h2>
          
          <div class="action-bar">
            <button class="btn-primary" on:click={() => activeView = 'addBook'}>Add New Book</button>
            <div class="search-controls">
              <input 
                type="text" 
                bind:value={searchQuery} 
                placeholder="Search books..."
                on:input={searchBooks}
              />
              <button class="btn-secondary" on:click={searchBooks}>Search</button>
            </div>
          </div>
          
          <div class="books-table">
            <table>
              <thead>
                <tr>
                  <th>Title</th>
                  <th>Author</th>
                  <th>ISBN</th>
                  <th>Category</th>
                  <th>Status</th>
                  <th>Actions</th>
                </tr>
              </thead>
              <tbody>
                {#each filteredBooks as book (book.id)}
                  <tr>
                    <td>{book.title}</td>
                    <td>{book.author}</td>
                    <td>{book.isbn}</td>
                    <td>{book.category}</td>
                    <td class={book.available ? 'available' : 'borrowed'}>
                      {book.available ? 'Available' : 'Borrowed'}
                    </td>
                    <td>
                      <button class="btn-small" on:click={() => viewBookDetails(book)}>View</button>
                      <button class="btn-small btn-secondary">Edit</button>
                      <button class="btn-small btn-danger">Delete</button>
                    </td>
                  </tr>
                {/each}
              </tbody>
            </table>
          </div>
        </div>
      
      <!-- Librarian: Add Book View -->
      {:else if activeView === 'addBook' && currentRole === 'librarian'}
        <div class="add-book">
          <h2>Add New Book</h2>
          
          <div class="form-container">
            <div class="form-group">
              <label for="title">Title</label>
              <input type="text" id="title" bind:value={newBookData.title} required />
            </div>
            
            <div class="form-group">
              <label for="author">Author</label>
              <input type="text" id="author" bind:value={newBookData.author} required />
            </div>
            
            <div class="form-group">
              <label for="isbn">ISBN</label>
              <input type="text" id="isbn" bind:value={newBookData.isbn} required />
            </div>
            
            <div class="form-row">
              <div class="form-group">
                <label for="publishYear">Publish Year</label>
                <input type="number" id="publishYear" bind:value={newBookData.publishYear} />
              </div>
              
              <div class="form-group">
                <label for="category">Category</label>
                <select id="category" bind:value={newBookData.category}>
                  <option value="Fiction">Fiction</option>
                  <option value="Non-Fiction">Non-Fiction</option>
                  <option value="Science Fiction">Science Fiction</option>
                  <option value="Fantasy">Fantasy</option>
                  <option value="Mystery">Mystery</option>
                  <option value="Biography">Biography</option>
                  <option value="History">History</option>
                  <option value="Science">Science</option>
                  <option value="Reference">Reference</option>
                  <option value="Classic">Classic</option>
                </select>
              </div>
            </div>
            
            <div class="form-group">
              <label for="description">Description</label>
              <textarea id="description" bind:value={newBookData.description} rows="4"></textarea>
            </div>
            
            <div class="form-actions">
              <button class="btn-secondary" on:click={() => activeView = 'manageBooks'}>Cancel</button>
              <button class="btn-primary" on:click={handleAddBook}>Add Book</button>
            </div>
          </div>
        </div>
      
      <!-- Librarian: Manage Users View -->
      {:else if activeView === 'manageUsers' && currentRole === 'librarian'}
        <div class="manage-users">
          <h2>Manage Users</h2>
          
          <div class="action-bar">
            <button class="btn-primary" on:click={() => activeView = 'addUser'}>Add New User</button>
          </div>
          
          <div class="users-table">
            <table>
              <thead>
                <tr>
                  <th>Name</th>
                  <th>Email</th>
                  <th>Role</th>
                  <th>Books Borrowed</th>
                  <th>Actions</th>
                </tr>
              </thead>
              <tbody>
                {#each users as user (user.id)}
                  <tr>
                    <td>{user.name}</td>
                    <td>{user.email}</td>
                    <td>{user.role}</td>
                    <td>{user.borrowedBooks.length}</td>
                    <td>
                      <button class="btn-small btn-secondary">Edit</button>
                      <button class="btn-small btn-danger">Delete</button>
                    </td>
                  </tr>
                {/each}
              </tbody>
            </table>
          </div>
        </div>
      
      <!-- Librarian: Add User View -->
      {:else if activeView === 'addUser' && currentRole === 'librarian'}
        <div class="add-user">
          <h2>Add New User</h2>
          
          <div class="form-container">
            <div class="form-group">
              <label for="name">Full Name</label>
              <input type="text" id="name" bind:value={newUserData.name} required />
            </div>
            
            <div class="form-group">
              <label for="email">Email</label>
              <input type="email" id="email" bind:value={newUserData.email} required />
            </div>
            
            <div class="form-group">
              <label for="role">Role</label>
              <select id="role" bind:value={newUserData.role}>
                <option value="student">Student</option>
                <option value="librarian">Librarian</option>
              </select>
            </div>
            
            <div class="form-actions">
              <button class="btn-secondary" on:click={() => activeView = 'manageUsers'}>Cancel</button>
              <button class="btn-primary" on:click={handleAddUser}>Add User</button>
            </div>
          </div>
        </div>
      
      <!-- Librarian: Loans View -->
      {:else if activeView === 'loans' && currentRole === 'librarian'}
        <div class="loans-management">
          <h2>Manage Loans</h2>
          
          <div class="loans-table">
            <table>
              <thead>
                <tr>
                  <th>Book Title</th>
                  <th>Borrowed By</th>
                  <th>Issue Date</th>
                  <th>Due Date</th>
                  <th>Status</th>
                  <th>Actions</th>
                </tr>
              </thead>
              <tbody>
                {#each loans as loan (loan.id)}
                  {@const book = books.find(b => b.id === loan.bookId)}
                  {@const user = users.find(u => u.id === loan.userId)}
                  <tr>
                    <td>{book.title}</td>
                    <td>{user.name}</td>
                    <td>{loan.issueDate}</td>
                    <td class={!loan.returned && new Date(loan.dueDate) < new Date() ? 'overdue' : ''}>
                      {loan.dueDate}
                    </td>
                    <td class={loan.returned ? 'returned' : 'active'}>
                      {loan.returned ? 'Returned' : 'Active'}
                    </td>
                    <td>
                      {#if !loan.returned}
                        <button 
                          class="btn-small" 
                          on:click={() => {
                            returnBook(loan.bookId);
                          }}
                        >
                          Mark Returned
                        </button>
                      {/if}
                    </td>
                  </tr>
                {/each}
              </tbody>
            </table>
          </div>
        </div>
      {/if}
    </div>
    
    <!-- Footer -->
    <footer>
      <p>&copy; 2023 School Library Management System</p>
    </footer>
  </main>
  
  <style>
    /* Base Styles */
    :global(body) {
      margin: 0;
      padding: 0;
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      line-height: 1.6;
      color: #333;
    }
    
    * {
      box-sizing: border-box;
    }
    
    main {
      display: flex;
      flex-direction: column;
      min-height: 100vh;
      background-color: #f5f5f5;
      transition: background-color 0.3s ease, color 0.3s ease;
    }
    
    main.dark-mode {
      background-color: #1a1a1a;
      color: #f0f0f0;
    }
    
    /* Header Styles */
    header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 1rem 2rem;
      background-color: #2c3e50;
      color: white;
      box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    }
    
    .dark-mode header {
      background-color: #121212;
    }
    
    .logo h1 {
      margin: 0;
      font-size: 1.5rem;
      font-weight: 700;
    }
    
    nav ul {
      display: flex;
      list-style: none;
      margin: 0;
      padding: 0;
      gap: 1rem;
    }
    
    nav button {
      background: none;
      border: none;
      color: #ddd;
      font-size: 1rem;
      cursor: pointer;
      padding: 0.5rem 1rem;
      border-radius: 4px;
      transition: background-color 0.2s;
    }
    
    nav button:hover {
      background-color: rgba(255, 255, 255, 0.1);
    }
    
    nav button.active {
      background-color: rgba(255, 255, 255, 0.2);
      color: white;
      font-weight: 600;
    }
    
    .user-controls {
      display: flex;
      align-items: center;
      gap: 1rem;
    }
    
    .user-name {
      font-weight: 600;
    }
    
    .user-role {
      font-size: 0.8rem;
      background-color: rgba(255, 255, 255, 0.2);
      padding: 0.2rem 0.5rem;
      border-radius: 12px;
      text-transform: capitalize;
    }
    
    .theme-toggle, .logout-btn {
      background: none;
      border: none;
      color: white;
      cursor: pointer;
      padding: 0.5rem;
      border-radius: 4px;
      transition: background-color 0.2s;
    }
    
    .theme-toggle {
      font-size: 1.2rem;
    }
    
    .theme-toggle:hover, .logout-btn:hover {
      background-color: rgba(255, 255, 255, 0.1);
    }
    
    .theme-toggle-container {
      margin-left: auto;
    }
    
    /* Content Styles */
    .content {
      flex: 1;
      padding: 2rem;
      max-width: 1200px;
      width: 100%;
      margin: 0 auto;
    }
    
    /* Auth Styles */
    .auth-container {
      display: flex;
      justify-content: center;
      align-items: center;
      min-height: 80vh;
    }
    
    .auth-form {
      background-color: white;
      padding: 2rem;
      border-radius: 8px;
      box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
      width: 100%;
      max-width: 400px;
    }
    
    .dark-mode .auth-form {
      background-color: #2a2a2a;
      box-shadow: 0 4px 6px rgba(0, 0, 0, 0.3);
    }
    
    .auth-form h2 {
      margin-top: 0;
      margin-bottom: 1.5rem;
      text-align: center;
    }
    
    .form-group {
      margin-bottom: 1rem;
    }
    
    .form-group label {
      display: block;
      margin-bottom: 0.5rem;
      font-weight: 600;
    }
    
    .form-group input, .form-group select, .form-group textarea {
      width: 100%;
      padding: 0.75rem;
      border: 1px solid #ddd;
      border-radius: 4px;
      font-size: 1rem;
      background-color: white;
    }
    
    .dark-mode .form-group input, 
    .dark-mode .form-group select, 
    .dark-mode .form-group textarea {
      background-color: #333;
      border-color: #444;
      color: #f0f0f0;
    }
    
    .form-row {
      display: flex;
      gap: 1rem;
    }
    
    .form-row .form-group {
      flex: 1;
    }
    
    .form-actions {
      display: flex;
      justify-content: flex-end;
      gap: 1rem;
      margin-top: 1.5rem;
    }
    
    .error-message {
      color: #e74c3c;
      margin-bottom: 1rem;
      padding: 0.5rem;
      background-color: rgba(231, 76, 60, 0.1);
      border-radius: 4px;
    }
    
    .demo-info {
      margin-top: 1.5rem;
      font-size: 0.9rem;
      color: #666;
      border-top: 1px solid #eee;
      padding-top: 1rem;
    }
    
    .dark-mode .demo-info {
      color: #aaa;
      border-top-color: #444;
    }
    
    /* Button Styles */
    .btn-primary, .btn-secondary, .btn-danger, .btn-small {
      padding: 0.75rem 1.5rem;
      border: none;
      border-radius: 4px;
      font-size: 1rem;
      font-weight: 600;
      cursor: pointer;
      transition: background-color 0.2s, transform 0.1s;
    }
    
    .btn-primary {
      background-color: #3498db;
      color: white;
    }
    
    .btn-primary:hover {
      background-color: #2980b9;
    }
    
    .btn-secondary {
      background-color: #95a5a6;
      color: white;
    }
    
    .btn-secondary:hover {
      background-color: #7f8c8d;
    }
    
    .btn-danger {
      background-color: #e74c3c;
      color: white;
    }
    
    .btn-danger:hover {
      background-color: #c0392b;
    }
    
    .btn-small {
      padding: 0.4rem 0.8rem;
      font-size: 0.9rem;
    }
    
    button:disabled {
      opacity: 0.6;
      cursor: not-allowed;
    }
    
    /* Dashboard Styles */
    .dashboard {
      display: flex;
      flex-direction: column;
      gap: 2rem;
    }
    
    .dashboard-stats {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
      gap: 1.5rem;
    }
    
    .stat-card {
      background-color: white;
      padding: 1.5rem;
      border-radius: 8px;
      box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    }
    
    .dark-mode .stat-card {
      background-color: #2a2a2a;
      box-shadow: 0 2px 4px rgba(0, 0, 0, 0.3);
    }
    
    .stat-card h3 {
      margin-top: 0;
      margin-bottom: 0.5rem;
      font-size: 1.1rem;
      color: #7f8c8d;
    }
    
    .dark-mode .stat-card h3 {
      color: #bdc3c7;
    }
    
    .stat-number {
      font-size: 2.5rem;
      font-weight: 700;
      margin: 0;
      color: #2c3e50;
    }
    
    .dark-mode .stat-number {
      color: #ecf0f1;
    }
    
    .quick-actions {
      background-color: white;
      padding: 1.5rem;
      border-radius: 8px;
      box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    }
    
    .dark-mode .quick-actions {
      background-color: #2a2a2a;
      box-shadow: 0 2px 4px rgba(0, 0, 0, 0.3);
    }
    
    .quick-actions h3 {
      margin-top: 0;
      margin-bottom: 1rem;
    }
    
    .action-buttons {
      display: flex;
      flex-wrap: wrap;
      gap: 1rem;
    }
    
    /* Books View Styles */
    .books-view {
      display: flex;
      flex-direction: column;
      gap: 1.5rem;
    }
    
    .search-container {
      background-color: white;
      padding: 1rem;
      border-radius: 8px;
      box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    }
    
    .dark-mode .search-container {
      background-color: #2a2a2a;
      box-shadow: 0 2px 4px rgba(0, 0, 0, 0.3);
    }
    
    .search-controls {
      display: flex;
      gap: 0.5rem;
    }
    
    .search-controls select {
      padding: 0.75rem;
      border: 1px solid #ddd;
      border-radius: 4px;
      font-size: 1rem;
      background-color: white;
    }
    
    .dark-mode .search-controls select {
      background-color: #333;
      border-color: #444;
      color: #f0f0f0;
    }
    
    .search-controls input {
      flex: 1;
      padding: 0.75rem;
      border: 1px solid #ddd;
      border-radius: 4px;
      font-size: 1rem;
    }
    
    .dark-mode .search-controls input {
      background-color: #333;
      border-color: #444;
      color: #f0f0f0;
    }
    
    .books-grid {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
      gap: 1.5rem;
    }
    
    .book-card {
      background-color: white;
      border-radius: 8px;
      overflow: hidden;
      box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
      transition: transform 0.2s, box-shadow 0.2s;
      cursor: pointer;
    }
    
    .dark-mode .book-card {
      background-color: #2a2a2a;
      box-shadow: 0 2px 4px rgba(0, 0, 0, 0.3);
    }
    
    .book-card:hover {
      transform: translateY(-5px);
      box-shadow: 0 4px 8px rgba(0, 0, 0, 0.15);
    }
    
    .book-cover {
      position: relative;
      height: 200px;
      overflow: hidden;
    }
    
    .book-cover img {
      width: 100%;
      height: 100%;
      object-fit: cover;
    }
    
    .book-status {
      position: absolute;
      top: 10px;
      right: 10px;
      padding: 0.3rem 0.6rem;
      border-radius: 12px;
      font-size: 0.8rem;
      font-weight: 600;
      background-color: #e74c3c;
      color: white;
    }
    
    .book-status.available {
      background-color: #2ecc71;
    }
    
    .book-info {
      padding: 1rem;
    }
    
    .book-title {
      margin: 0 0 0.5rem 0;
      font-size: 1.1rem;
      line-height: 1.3;
    }
    
    .book-author {
      margin: 0 0 0.5rem 0;
      color: #7f8c8d;
      font-size: 0.9rem;
    }
    
    .dark-mode .book-author {
      color: #bdc3c7;
    }
    
    .book-category {
      display: inline-block;
      padding: 0.2rem 0.5rem;
      background-color: #f0f0f0;
      border-radius: 12px;
      font-size: 0.8rem;
      color: #555;
    }
    
    .dark-mode .book-category {
      background-color: #333;
      color: #ddd;
    }
    
    .no-results {
      grid-column: 1 / -1;
      text-align: center;
      padding: 2rem;
      background-color: white;
      border-radius: 8px;
      box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    }
    
    .dark-mode .no-results {
      background-color: #2a2a2a;
      box-shadow: 0 2px 4px rgba(0, 0, 0, 0.3);
    }
    
    /* Book Details Styles */
    .book-details {
      background-color: white;
      border-radius: 8px;
      padding: 2rem;
      box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    }
    
    .dark-mode .book-details {
      background-color: #2a2a2a;
      box-shadow: 0 2px 4px rgba(0, 0, 0, 0.3);
    }
    
    .back-button {
      background: none;
      border: none;
      color: #3498db;
      cursor: pointer;
      padding: 0;
      font-size: 1rem;
      margin-bottom: 1.5rem;
      display: inline-block;
    }
    
    .dark-mode .back-button {
      color: #5dade2;
    }
    
    .book-details-content {
      display: grid;
      grid-template-columns: 250px 1fr;
      gap: 2rem;
    }
    
    @media (max-width: 768px) {
      .book-details-content {
        grid-template-columns: 1fr;
      }
    }
    
    .book-cover-large {
      width: 100%;
      max-width: 250px;
      box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
      border-radius: 8px;
      overflow: hidden;
    }
    
    .book-cover-large img {
      width: 100%;
      height: auto;
      display: block;
    }
    
    .book-info-detailed h2 {
      margin-top: 0;
      margin-bottom: 0.5rem;
      font-size: 1.8rem;
    }
    
    .book-info-detailed .author {
      color: #7f8c8d;
      margin-bottom: 1.5rem;
      font-size: 1.1rem;
    }
    
    .dark-mode .book-info-detailed .author {
      color: #bdc3c7;
    }
    
    .book-metadata {
      margin-bottom: 1.5rem;
      line-height: 1.8;
    }
    
    .availability {
      color: #e74c3c;
    }
    
    .availability.available {
      color: #2ecc71;
    }
    
    .book-description h3 {
      margin-bottom: 0.5rem;
    }
    
    .book-description p {
      line-height: 1.6;
    }
    
    .book-actions {
      margin-top: 2rem;
      display: flex;
      gap: 1rem;
    }
    
    /* Table Styles */
    .loans-table, .books-table, .users-table {
      width: 100%;
      overflow-x: auto;
      background-color: white;
      border-radius: 8px;
      box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    }
    
    .dark-mode .loans-table, 
    .dark-mode .books-table, 
    .dark-mode .users-table {
      background-color: #2a2a2a;
      box-shadow: 0 2px 4px rgba(0, 0, 0, 0.3);
    }
    
    table {
      width: 100%;
      border-collapse: collapse;
    }
    
    th, td {
      padding: 1rem;
      text-align: left;
      border-bottom: 1px solid #eee;
    }
    
    .dark-mode th, .dark-mode td {
      border-bottom-color: #444;
    }
    
    th {
      background-color: #f8f9fa;
      font-weight: 600;
    }
    
    .dark-mode th {
      background-color: #333;
    }
    
    tr:last-child td {
      border-bottom: none;
    }
    
    .overdue {
      color: #e74c3c;
      font-weight: 600;
    }
    
    .available, .returned {
      color: #2ecc71;
    }
    
    .borrowed, .active {
      color: #f39c12;
    }
    
    /* Manage Books/Users Styles */
    .manage-books, .manage-users, .loans-management, .my-books {
      display: flex;
      flex-direction: column;
      gap: 1.5rem;
    }
    
    .action-bar {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 1rem;
    }
    
    /* Form Container */
    .form-container {
      background-color: white;
      padding: 2rem;
      border-radius: 8px;
      box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    }
    
    .dark-mode .form-container {
      background-color: #2a2a2a;
      box-shadow: 0 2px 4px rgba(0, 0, 0, 0.3);
    }
    
    /* Footer Styles */
    footer {
      background-color: #2c3e50;
      color: white;
      text-align: center;
      padding: 1rem;
      margin-top: 2rem;
    }
    
    .dark-mode footer {
      background-color: #121212;
    }
    
    /* Responsive Adjustments */
    @media (max-width: 768px) {
      header {
        flex-direction: column;
        padding: 1rem;
      }
      
      nav ul {
        margin-top: 1rem;
        flex-wrap: wrap;
        justify-content: center;
      }
      
      .user-controls {
        margin-top: 1rem;
        flex-wrap: wrap;
        justify-content: center;
      }
      
      .content {
        padding: 1rem;
      }
      
      .dashboard-stats {
        grid-template-columns: 1fr;
      }
      
      .books-grid {
        grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
      }
      
      .book-details-content {
        grid-template-columns: 1fr;
      }
      
      .book-cover-large {
        margin: 0 auto 1.5rem;
      }
      
      .form-row {
        flex-direction: column;
        gap: 0;
      }
    }
  </style>