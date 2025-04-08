<script>
    import { onMount } from 'svelte';
    import { fade } from 'svelte/transition';
  
    let users = [];
    let isLoading = false;
    let error = null;
    let searchTerm = '';
    let selectedUser = null;
    let showUserModal = false;
    let showDeleteModal = false;
    let formData = {
      id: '',
      name: '',
      email: '',
      role: 'user',
      active: true
    };
  
    const roles = [
      { value: 'admin', label: 'Administrator' },
      { value: 'editor', label: 'Editor' },
      { value: 'user', label: 'Standard User' }
    ];
  
    async function fetchUsers() {
      isLoading = true;
      error = null;
      try {
        users = [
          { id: 1, name: 'Admin User', email: 'admin@example.com', role: 'admin', active: true },
          { id: 2, name: 'Editor User', email: 'editor@example.com', role: 'editor', active: true },
          { id: 3, name: 'Regular User', email: 'user@example.com', role: 'user', active: false }
        ];
      } catch (err) {
        error = 'Failed to load users. Please try again.';
        console.error(err);
      } finally {
        isLoading = false;
      }
    }
  
    $: filteredUsers = users.filter(user => 
      user.name.toLowerCase().includes(searchTerm.toLowerCase()) ||
      user.email.toLowerCase().includes(searchTerm.toLowerCase())
    );
  
    onMount(() => {
      fetchUsers();
    });
  
    function editUser(user) {
      selectedUser = user;
      formData = { ...user };
      showUserModal = true;
    }
  
    function confirmDelete(user) {
      selectedUser = user;
      showDeleteModal = true;
    }
  
    async function saveUser() {
      isLoading = true;
      try {
        if (formData.id) {
          const index = users.findIndex(u => u.id === formData.id);
          users[index] = { ...formData };
        } else {
          const newUser = { ...formData, id: Math.max(...users.map(u => u.id)) + 1 };
          users = [...users, newUser];
        }
        
        showUserModal = false;
      } catch (err) {
        error = 'Failed to save user. Please try again.';
        console.error(err);
      } finally {
        isLoading = false;
      }
    }
  
    async function deleteUser() {
      isLoading = true;
      try {
        users = users.filter(user => user.id !== selectedUser.id);
        showDeleteModal = false;
        selectedUser = null;
      } catch (err) {
        error = 'Failed to delete user. Please try again.';
        console.error(err);
      } finally {
        isLoading = false;
      }
    }
  
    function addNewUser() {
      formData = {
        id: '',
        name: '',
        email: '',
        role: 'user',
        active: true
      };
      selectedUser = null;
      showUserModal = true;
    }
  </script>
  
  <div class="admin-settings">
    <h1>User Management</h1>
  
    {#if error}
      <div class="alert alert-error">
        <span>{error}</span>
        <button class="close" on:click={() => error = null}>✖</button>
      </div>
    {/if}
  
    <div class="controls">
      <div class="search-box">
        <input 
          type="text" 
          bind:value={searchTerm} 
          placeholder="Search users..." 
          disabled={isLoading}
        />
        <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor">
          <circle cx="11" cy="11" r="8" />
          <path d="M21 21l-4.35-4.35" />
        </svg>
      </div>
  
      <button on:click={addNewUser} class="btn-primary" disabled={isLoading}>
        {isLoading ? 'Loading...' : 'Add New User'}
      </button>
    </div>
  
    {#if isLoading && users.length === 0}
      <div class="loading">Loading users...</div>
    {:else if filteredUsers.length === 0}
      <div class="empty-state">No users found</div>
    {:else}
      <table class="user-table">
        <thead>
          <tr>
            <th>Name</th>
            <th>Email</th>
            <th>Role</th>
            <th>Status</th>
            <th>Actions</th>
          </tr>
        </thead>
        <tbody>
          {#each filteredUsers as user (user.id)}
            <tr>
              <td>{user.name}</td>
              <td>{user.email}</td>
              <td>
                <span class={`role-badge ${user.role}`}>
                  {roles.find(r => r.value === user.role)?.label || user.role}
                </span>
              </td>
              <td>
                <span class={`status ${user.active ? 'active' : 'inactive'}`}>
                  {user.active ? 'Active' : 'Inactive'}
                </span>
              </td>
              <td class="actions">
                <button on:click={() => editUser(user)} class="btn-icon" title="Edit">
                  <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor">
                    <path d="M11 4H4a2 2 0 0 0-2 2v14a2 2 0 0 0 2 2h14a2 2 0 0 0 2-2v-7" />
                    <path d="M18.5 2.5a2.121 2.121 0 0 1 3 3L12 15l-4 1 1-4 9.5-9.5z" />
                  </svg>
                </button>
                <button on:click={() => confirmDelete(user)} class="btn-icon danger" title="Delete">
                  <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor">
                    <path d="M3 6h18" />
                    <path d="M19 6v14a2 2 0 0 1-2 2H7a2 2 0 0 1-2-2V6m3 0V4a2 2 0 0 1 2-2h4a2 2 0 0 1 2 2v2" />
                  </svg>
                </button>
              </td>
            </tr>
          {/each}
        </tbody>
      </table>
    {/if}
  
    {#if showUserModal}
      <div 
        class="modal-backdrop" 
        on:click|self={() => showUserModal = false} 
        on:keydown|self={(e) => { if (e.key === 'Escape') showUserModal = false; }}
      >
        <div class="modal" transition:fade>
          <h2>{selectedUser ? 'Edit User' : 'Add New User'}</h2>
          
          <form on:submit|preventDefault={saveUser}>
            <div class="form-group">
              <label for="name">Full Name</label>
              <input 
                id="name" 
                type="text" 
                bind:value={formData.name} 
                required 
                disabled={isLoading}
              />
            </div>
  
            <div class="form-group">
              <label for="email">Email</label>
              <input 
                id="email" 
                type="email" 
                bind:value={formData.email} 
                required 
                disabled={isLoading}
              />
            </div>
  
            <div class="form-group">
              <label for="role">Role</label>
              <select id="role" bind:value={formData.role} disabled={isLoading}>
                {#each roles as role}
                  <option value={role.value}>{role.label}</option>
                {/each}
              </select>
            </div>
  
            <div class="form-group checkbox-group">
              <input 
                id="active" 
                type="checkbox" 
                bind:checked={formData.active} 
                disabled={isLoading}
              />
              <label for="active">Active User</label>
            </div>
  
            <div class="form-actions">
              <button 
                type="button" 
                on:click={() => showUserModal = false} 
                class="btn-secondary" 
                disabled={isLoading}
              >
                Cancel
              </button>
              <button type="submit" class="btn-primary" disabled={isLoading}>
                {isLoading ? 'Saving...' : 'Save User'}
              </button>
            </div>
          </form>
        </div>
      </div>
    {/if}
    {#if showDeleteModal}
      <div 
        class="modal-backdrop" 
        on:click|self={() => showDeleteModal = false} 
        on:keydown|self={(e) => { if (e.key === 'Escape') showDeleteModal = false; }}
      >
          <h2>Confirm Deletion</h2>
          <p>Are you sure you want to delete user <strong>{selectedUser?.name}</strong>? This action cannot be undone.</p>
          
          <div class="form-actions">
            <button 
              type="button" 
              on:click={() => showDeleteModal = false} 
              class="btn-secondary" 
              disabled={isLoading}
            >
              Cancel
            </button>
            <button 
              type="button" 
              on:click={deleteUser} 
              class="btn-danger" 
              disabled={isLoading}
            >
              {isLoading ? 'Deleting...' : 'Delete User'}
            </button>
        </div>
      </div>
    {/if}
  </div>
  
  <style>
    .admin-settings {
      padding: 1.5rem;
      max-width: 1200px;
      margin: 0 auto;
    }
  
    h1 {
      margin-bottom: 1.5rem;
      font-size: 1.8rem;
      color: #333;
    }
  
    .alert {
      padding: 0.75rem 1rem;
      border-radius: 4px;
      margin-bottom: 1.5rem;
      display: flex;
      justify-content: space-between;
      align-items: center;
    }
  
    .alert-error {
      background-color: #fee2e2;
      color: #b91c1c;
      border: 1px solid #fca5a5;
    }
  
    .close {
      background: none;
      border: none;
      color: inherit;
      font-size: 1.25rem;
      cursor: pointer;
      padding: 0 0.5rem;
    }
  
    .controls {
      display: flex;
      justify-content: space-between;
      margin-bottom: 1.5rem;
      gap: 1rem;
      flex-wrap: wrap;
    }
  
    .search-box {
      position: relative;
      flex: 1;
      max-width: 400px;
    }
  
    .search-box input {
      width: 100%;
      padding: 0.5rem 2rem 0.5rem 1rem;
      border: 1px solid #d1d5db;
      border-radius: 4px;
    }
  
    .search-box svg {
      position: absolute;
      right: 0.75rem;
      top: 50%;
      transform: translateY(-50%);
      pointer-events: none;
    }
  
    .loading, .empty-state {
      text-align: center;
      padding: 2rem;
      color: #6b7280;
    }
  
    .user-table {
      width: 100%;
      border-collapse: collapse;
      background: white;
      box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
      border-radius: 8px;
      overflow: hidden;
    }
  
    .user-table th, .user-table td {
      padding: 1rem;
      text-align: left;
      border-bottom: 1px solid #e5e7eb;
    }
  
    .user-table th {
      background-color: #f9fafb;
      font-weight: 600;
      color: #374151;
    }
  
    .user-table tr:last-child td {
      border-bottom: none;
    }
  
    .role-badge {
      display: inline-block;
      padding: 0.25rem 0.5rem;
      border-radius: 9999px;
      font-size: 0.75rem;
      font-weight: 500;
      text-transform: capitalize;
    }
  
    .role-badge.admin {
      background-color: #fef2f2;
      color: #b91c1c;
    }
  
    .role-badge.editor {
      background-color: #eff6ff;
      color: #1d4ed8;
    }
  
    .role-badge.user {
      background-color: #ecfdf5;
      color: #047857;
    }
  
    .status {
      display: inline-block;
      padding: 0.25rem 0.5rem;
      border-radius: 9999px;
      font-size: 0.75rem;
      font-weight: 500;
    }
  
    .status.active {
      background-color: #dcfce7;
      color: #166534;
    }
  
    .status.inactive {
      background-color: #fee2e2;
      color: #991b1b;
    }
  
    .actions {
      display: flex;
      gap: 0.5rem;
    }
  
    .btn-icon {
      display: flex;
      align-items: center;
      justify-content: center;
      width: 2rem;
      height: 2rem;
      border-radius: 4px;
      background: none;
      border: 1px solid #d1d5db;
      cursor: pointer;
      color: #6b7280;
    }
  
    .btn-icon:hover {
      background-color: #f3f4f6;
      color: #374151;
    }
  
    .btn-icon.danger {
      color: #ef4444;
      border-color: #fecaca;
    }
  
    .btn-icon.danger:hover {
      background-color: #fee2e2;
      color: #b91c1c;
    }
  
    .btn-primary {
      background-color: #3b82f6;
      color: white;
      border: none;
      padding: 0.5rem 1rem;
      border-radius: 4px;
      cursor: pointer;
      font-weight: 500;
    }
  
    .btn-primary:hover {
      background-color: #2563eb;
    }
  
    .btn-primary:disabled {
      background-color: #bfdbfe;
      cursor: not-allowed;
    }
  
    .btn-secondary {
      background-color: white;
      color: #374151;
      border: 1px solid #d1d5db;
      padding: 0.5rem 1rem;
      border-radius: 4px;
      cursor: pointer;
      font-weight: 500;
    }
  
    .btn-secondary:hover {
      background-color: #f9fafb;
    }
  
    .btn-secondary:disabled {
      opacity: 0.7;
      cursor: not-allowed;
    }
  
    .btn-danger {
      background-color: #ef4444;
      color: white;
      border: none;
      padding: 0.5rem 1rem;
      border-radius: 4px;
      cursor: pointer;
      font-weight: 500;
    }
  
    .btn-danger:hover {
      background-color: #dc2626;
    }
  
    .btn-danger:disabled {
      background-color: #fca5a5;
      cursor: not-allowed;
    }
  
    .modal-backdrop {
      position: fixed;
      top: 0;
      left: 0;
      right: 0;
      bottom: 0;
      background-color: rgba(0, 0, 0, 0.5);
      display: flex;
      justify-content: center;
      align-items: center;
      z-index: 1000;
    }
  
    .modal {
      background-color: white;
      border-radius: 8px;
      padding: 1.5rem;
      width: 100%;
      max-width: 500px;
      box-shadow: 0 10px 25px rgba(0, 0, 0, 0.1);
    }
  
    .modal-sm {
      max-width: 400px;
    }
  
    .modal h2 {
      margin-top: 0;
      margin-bottom: 1.5rem;
      font-size: 1.25rem;
    }
  
    .form-group {
      margin-bottom: 1rem;
    }
  
    .form-group label {
      display: block;
      margin-bottom: 0.5rem;
      font-weight: 500;
      color: #374151;
    }
  
    .form-group input[type="text"],
    .form-group input[type="email"],
    .form-group select {
      width: 100%;
      padding: 0.5rem;
      border: 1px solid #d1d5db;
      border-radius: 4px;
    }
  
    .checkbox-group {
      display: flex;
      align-items: center;
      gap: 0.5rem;
    }
  
    .checkbox-group input {
      margin: 0;
    }
  
    .form-actions {
      display: flex;
      justify-content: flex-end;
      gap: 0.5rem;
      margin-top: 1.5rem;
    }
  </style>