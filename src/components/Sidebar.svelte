<script>
  export let currentView;
  export let handleNavigation;

  const menuItems = [
    { id: "dashboard", label: "Dashboard", icon: "📊" },
    { id: "students", label: "Students", icon: "👨‍🎓" },
    { id: "teachers", label: "Teachers", icon: "👩‍🏫" },
    { id: "classes", label: "Classes", icon: "🏫" },
    { id: "attendance", label: "Attendance", icon: "📋" },
    { id: "exams", label: "Exams", icon: "📝" },
    { id: "reports", label: "Reports", icon: "📊" },
    { id: "settings", label: "Settings", icon: "⚙️" },
  ];
</script>

<aside class="sidebar">
  <div class="logo">
    <h2>School MS</h2>
  </div>

  <nav>
    <ul>
      {#each menuItems as item}
        <li
          class={currentView === item.id ? "active" : ""}
          on:click={() => handleNavigation(item.id)}
          on:keydown={(e) => e.key === 'Enter' && handleNavigation(item.id)}
        >
          <span class="icon">{item.icon}</span>
          <span class="label">{item.label}</span>
        </li>
      {/each}
    </ul>
  </nav>

  <div class="sidebar-footer">
    <button class="logout-btn">Logout</button>
  </div>
</aside>

<style>
  .sidebar {
    width: 250px;
    background-color: #2c3e50;
    color: #ecf0f1;
    display: flex;
    flex-direction: column;
    height: 100%;
    box-shadow: 2px 0 5px rgba(0, 0, 0, 0.1);
  }

  .logo {
    padding: 20px;
    text-align: center;
    border-bottom: 1px solid #34495e;
    font-size: 24px;
    font-weight: bold;
    color: #ecf0f1;
  }

  nav {
    flex: 1;
    overflow-y: auto;
  }

  ul {
    list-style-type: none;
    padding: 0;
    margin: 0;
  }

  li {
    padding: 15px 20px;
    cursor: pointer;
    transition:
      background-color 0.3s,
      transform 0.2s;
    display: flex;
    align-items: center;
    border-left: 4px solid transparent;
  }

  li:hover {
    background-color: #34495e;
    transform: translateX(5px);
  }

  li.active {
    background-color: #3498db;
    border-left: 4px solid #2980b9;
  }

  .icon {
    margin-right: 10px;
    font-size: 18px;
  }

  .label {
    font-size: 16px;
  }

  .sidebar-footer {
    padding: 20px;
    border-top: 1px solid #34495e;
  }

  .logout-btn {
    width: 100%;
    padding: 10px;
    background-color: #e74c3c;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    transition: background-color 0.3s;
  }

  .logout-btn:hover {
    background-color: #c0392b;
  }
</style>
