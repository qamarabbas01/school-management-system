<script>
  export let user;
  
  let currentDate = new Date().toLocaleDateString('en-US', {
    weekday: 'long',
    year: 'numeric',
    month: 'long',
    day: 'numeric'
  });
</script>

<header class="header">
  <div class="search-bar">
    <input type="text" placeholder="Search..." />
    <button class="search-btn">🔍</button>
  </div>
  
  <div class="header-right">
    <div class="date">{currentDate}</div>
    <div class="notifications">
      <span class="icon">🔔</span>
      <span class="badge">3</span>
    </div>
    <div class="user-info">
      <span class="avatar">👤</span>
      <div class="user-details">
        <div class="name">{user.name}</div>
        <div class="role">{user.role}</div>
      </div>
    </div>
  </div>
</header>

<style>
  .header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 15px;
    background-color: white;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    margin-bottom: 20px;
  }
  
  .search-bar {
    display: flex;
    align-items: center;
  }
  
  .search-bar input {
    padding: 10px;
    border: 1px solid #ddd;
    border-radius: 4px 0 0 4px;
    min-width: 300px;
  }
  
  .search-btn {
    padding: 10px 15px;
    background-color: #3498db;
    color: white;
    border: none;
    border-radius: 0 4px 4px 0;
    cursor: pointer;
  }
  
  .header-right {
    display: flex;
    align-items: center;
    gap: 20px;
  }
  
  .date {
    font-size: 14px;
    color: #7f8c8d;
  }
  
  .notifications {
    position: relative;
    cursor: pointer;
  }
  
  .notifications .icon {
    font-size: 18px;
  }
  
  .badge {
    position: absolute;
    top: -5px;
    right: -5px;
    background-color: #e74c3c;
    color: white;
    border-radius: 50%;
    width: 16px;
    height: 16px;
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 10px;
  }
  
  .user-info {
    display: flex;
    align-items: center;
    gap: 10px;
    cursor: pointer;
  }
  
  .avatar {
    font-size: 24px;
  }
  
  .user-details .name {
    font-weight: bold;
  }
  
  .user-details .role {
    font-size: 12px;
    color: #7f8c8d;
  }
</style>