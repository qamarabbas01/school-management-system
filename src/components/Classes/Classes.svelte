<script>
    let classes = [
      { 
        id: 1, 
        name: "Mathematics 101", 
        department: "Mathematics",
        schedule: "Mon, Wed, Fri 9:00 AM - 10:30 AM",
        room: "A-201",
        teacher: "Dr. Alan Turing",
        capacity: 30,
        enrolled: 24,
        description: "Introduction to calculus and algebraic concepts."
      },
      { 
        id: 2, 
        name: "Introduction to Physics", 
        department: "Science",
        schedule: "Tue, Thu 11:00 AM - 12:30 PM",
        room: "B-105",
        teacher: "Dr. Marie Curie",
        capacity: 25,
        enrolled: 22,
        description: "Fundamentals of classical mechanics and thermodynamics."
      },
      { 
        id: 3, 
        name: "World History", 
        department: "History",
        schedule: "Mon, Wed 2:00 PM - 3:30 PM",
        room: "C-310",
        teacher: "Prof. Howard Zinn",
        capacity: 35,
        enrolled: 28,
        description: "Survey of major historical events and their global impact."
      },
      { 
        id: 4, 
        name: "English Literature", 
        department: "English",
        schedule: "Tue, Thu 1:00 PM - 2:30 PM",
        room: "D-112",
        teacher: "Dr. Jane Austen",
        capacity: 20,
        enrolled: 18,
        description: "Analysis of classic and contemporary literary works."
      }
    ];
    
    let departments = ["Mathematics", "Science", "History", "English", "Computer Science", "Art", "Music", "Physical Education"];
    
    let teachers = [
      "Dr. Alan Turing",
      "Dr. Marie Curie",
      "Prof. Howard Zinn",
      "Dr. Jane Austen",
      "Prof. Albert Einstein",
      "Dr. Grace Hopper",
      "Prof. Richard Feynman",
      "Dr. Ada Lovelace"
    ];
    
    let activeView = "list";
    let selectedClass = null;
    let searchQuery = "";
    let filterDepartment = "All";
    
    let formData = {
      id: null,
      name: "",
      department: "",
      schedule: "",
      room: "",
      teacher: "",
      capacity: 30,
      enrolled: 0,
      description: ""
    };
    
    $: filteredClasses = classes.filter(cls => {
      const matchesSearch = cls.name.toLowerCase().includes(searchQuery.toLowerCase()) || 
                            cls.teacher.toLowerCase().includes(searchQuery.toLowerCase());
      const matchesDepartment = filterDepartment === "All" || cls.department === filterDepartment;
      return matchesSearch && matchesDepartment;
    });
    
    function viewClassDetails(cls) {
      selectedClass = cls;
      activeView = "details";
    }
    
    function editClass(cls) {
      formData = { ...cls };
      activeView = "edit";
    }
    
    function createNewClass() {
      formData = {
        id: classes.length > 0 ? Math.max(...classes.map(c => c.id)) + 1 : 1,
        name: "",
        department: departments[0],
        schedule: "",
        room: "",
        teacher: teachers[0],
        capacity: 30,
        enrolled: 0,
        description: ""
      };
      activeView = "create";
    }
    
    function saveClass() {
      if (activeView === "edit") {
        const index = classes.findIndex(c => c.id === formData.id);
        if (index !== -1) {
          classes[index] = { ...formData };
          classes = [...classes];
        }
      } else if (activeView === "create") {
        classes = [...classes, { ...formData }];
      }
      
      activeView = "list";
    }
    
    function deleteClass(id) {
      if (confirm("Are you sure you want to delete this class?")) {
        classes = classes.filter(c => c.id !== id);
        activeView = "list";
      }
    }
    
    function cancelEdit() {
      activeView = selectedClass ? "details" : "list";
    }
    
    function getCapacityColorClass(enrolled, capacity) {
      const ratio = enrolled / capacity;
      if (ratio >= 0.9) return "text-red-600";
      if (ratio >= 0.75) return "text-amber-500";
      return "text-green-600";
    }
  </script>
  
  <div class="school-management">
    <header class="header">
      <h1>School Management System</h1>
      <p>Manage classes, enrollments, and schedules</p>
    </header>
    
    <nav class="main-nav">
      <div class="nav-actions">
        <button 
          class:active={activeView === 'list'}
          on:click={() => activeView = 'list'}
        >
          Classes
        </button>
        <button 
          class="btn-primary"
          on:click={createNewClass}
        >
          Add New Class
        </button>
        </div>
      
      {#if activeView === 'list'}
        <div class="search-filters">
          <input 
            type="text" 
            placeholder="Search classes or teachers..." 
            bind:value={searchQuery}
            class="search-input"
          />
          <select
            bind:value={filterDepartment}
            class="department-select"
          >
            <option value="All">All Departments</option>
            {#each departments as dept}
              <option value={dept}>{dept}</option>
            {/each}
          </select>
        </div>
      {/if}
    </nav>
    
    <main class="main-content">
      {#if activeView === 'list'}
        <div class="class-grid">
            {#each filteredClasses as cls}
            <button
              class="class-card"
              on:click={() => viewClassDetails(cls)}
              on:keydown={(e) => e.key === 'Enter' && viewClassDetails(cls)}
            >
             See Details
            </button>
              <div class="class-card-header">
                <h3>{cls.name}</h3>
                <p>{cls.department}</p>
              </div>
              <div class="class-card-body">
                <div class="class-info-grid">
                  <div>
                    <span>Teacher:</span>
                    <p>{cls.teacher}</p>
                  </div>
                  <div>
                    <span>Room:</span>
                    <p>{cls.room}</p>
                  </div>
                  <div>
                    <span>Schedule:</span>
                    <p class="schedule-text">{cls.schedule}</p>
                  </div>
                  <div>
                    <span>Enrollment:</span>
                    <p class={getCapacityColorClass(cls.enrolled, cls.capacity)}>
                      {cls.enrolled}/{cls.capacity}
                    </p>
                  </div>
                </div>
              </div>

          {/each}
          
          {#if filteredClasses.length === 0}
            <div class="no-results">
              <p>No classes found matching your criteria.</p>
              <button 
                class="btn-clear-filters"
                on:click={() => { searchQuery = ""; filterDepartment = "All"; }}
              >
                Clear filters
              </button>
            </div>
          {/if}
        </div>
      {/if}
      
      {#if activeView === 'details' && selectedClass}
        <div class="class-detail-container">
          <div class="class-detail-header">
            <div>
              <h2>{selectedClass.name}</h2>
              <p>{selectedClass.department}</p>
            </div>
            <div class="detail-actions">
              <button 
                class="btn-primary"
                on:click={() => editClass(selectedClass)}
              >
                Edit Class
              </button>
              <button 
                class="btn-danger"
                on:click={() => deleteClass(selectedClass.id)}
              >
                Delete
              </button>
            </div>
          </div>
          
          <div class="class-detail-body">
            <div>
              <h3>Class Information</h3>
              <div class="detail-info-grid">
                <div>
                  <span>Teacher:</span>
                  <p>{selectedClass.teacher}</p>
                </div>
                <div>
                  <span>Schedule:</span>
                  <p>{selectedClass.schedule}</p>
                </div>
                <div>
                  <span>Room:</span>
                  <p>{selectedClass.room}</p>
                </div>
                <div>
                  <span>Enrollment:</span>
                  <p class={getCapacityColorClass(selectedClass.enrolled, selectedClass.capacity)}>
                    {selectedClass.enrolled} students enrolled (Capacity: {selectedClass.capacity})
                  </p>
                  <div class="enrollment-bar">
                    <div 
                      class="enrollment-progress" 
                      style="width: {(selectedClass.enrolled / selectedClass.capacity) * 100}%"
                    ></div>
                  </div>
                </div>
              </div>
            </div>
            
            <div>
              <h3>Description</h3>
              <p class="class-description">{selectedClass.description || "No description available."}</p>
              
              <h3>Actions</h3>
              <div class="action-buttons">
                <button class="btn-secondary">
                  View Student List
                </button>
                <button class="btn-secondary">
                  Manage Enrollment
                </button>
                <button class="btn-secondary">
                  View Schedule
                </button>
                <button class="btn-secondary">
                  Print Class Roster
                </button>
              </div>
            </div>
          </div>
          
          <div class="class-detail-footer">
            <button 
              class="btn-back"
              on:click={() => activeView = 'list'}
            >
              Back to Classes
            </button>
          </div>
        </div>
      {/if}
      
      {#if activeView === 'edit' || activeView === 'create'}
        <div class="form-container">
          <div class="form-header">
            <h2>{activeView === 'edit' ? 'Edit Class' : 'Create New Class'}</h2>
          </div>
          
          <form class="class-form" on:submit|preventDefault={saveClass}>
            <div class="form-grid">
              <div class="form-column">
                <div class="form-group">
                  <label for="name">Class Name</label>
                  <input 
                    type="text" 
                    id="name" 
                    bind:value={formData.name} 
                    required
                  />
                </div>
                
                <div class="form-group">
                  <label for="department">Department</label>
                  <select 
                    id="department" 
                    bind:value={formData.department} 
                    required
                  >
                    {#each departments as dept}
                      <option value={dept}>{dept}</option>
                    {/each}
                  </select>
                </div>
                
                <div class="form-group">
                  <label for="teacher">Teacher</label>
                  <select 
                    id="teacher" 
                    bind:value={formData.teacher} 
                    required
                  >
                    {#each teachers as teacher}
                      <option value={teacher}>{teacher}</option>
                    {/each}
                  </select>
                </div>
                
                <div class="form-group">
                  <label for="schedule">Schedule</label>
                  <input 
                    type="text" 
                    id="schedule" 
                    bind:value={formData.schedule} 
                    placeholder="e.g. Mon, Wed, Fri 9:00 AM - 10:30 AM"
                    required
                  />
                </div>
              </div>
              
              <div class="form-column">
                <div class="form-group">
                  <label for="room">Room</label>
                  <input 
                    type="text" 
                    id="room" 
                    bind:value={formData.room} 
                    required
                  />
                </div>
                
                <div class="form-group">
                  <label for="capacity">Capacity</label>
                  <input 
                    type="number" 
                    id="capacity" 
                    bind:value={formData.capacity} 
                    min="1"
                    max="100"
                    required
                  />
                </div>
                
                <div class="form-group">
                  <label for="enrolled">Currently Enrolled</label>
                  <input 
                    type="number" 
                    id="enrolled" 
                    bind:value={formData.enrolled} 
                    min="0"
                    max={formData.capacity}
                    required
                  />
                </div>
                
                <div class="form-group">
                  <label for="description">Description</label>
                  <textarea 
                    id="description" 
                    bind:value={formData.description} 
                    rows="3"
                  ></textarea>
                </div>
              </div>
            </div>
            
            <div class="form-actions">
              <button 
                type="button"
                class="btn-cancel"
                on:click={cancelEdit}
              >
                Cancel
              </button>
              <button 
                type="submit"
                class="btn-save"
              >
                {activeView === 'edit' ? 'Save Changes' : 'Create Class'}
              </button>
            </div>
          </form>
        </div>
      {/if}
    </main>
  </div>
  
  <style>
    :root {
      --primary: #4f46e5;
      --primary-hover: #4338ca;
      --primary-light: #e0e7ff;
      --secondary: #64748b;
      --secondary-hover: #475569;
      --danger: #ef4444;
      --danger-hover: #dc2626;
      --success: #10b981;
      --warning: #f59e0b;
      --background: #f8fafc;
      --card-bg: #ffffff;
      --text: #1e293b;
      --text-light: #64748b;
      --border: #e2e8f0;
      --border-dark: #cbd5e1;
      --radius: 0.5rem;
      --shadow-sm: 0 1px 2px 0 rgba(0, 0, 0, 0.05);
      --shadow-md: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
      --shadow-lg: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -2px rgba(0, 0, 0, 0.05);
      --transition: all 0.2s ease;
    }
    
    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }

    
    .school-management {
      max-width: 1200px;
      margin: 0 auto;
      padding: 2rem;
      min-height: 100vh;
    }
    
    .header {
      margin-bottom: 2rem;
      text-align: center;
    }
    
    .header h1 {
      font-size: 2.25rem;
      font-weight: 800;
      color: var(--primary);
      margin-bottom: 0.5rem;
    }
    
    .header p {
      color: var(--secondary);
      font-size: 1.125rem;
    }
    
    .main-nav {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 2rem;
      padding-bottom: 1rem;
      border-bottom: 1px solid var(--border);
    }
    
    .nav-actions {
      display: flex;
      gap: 1rem;
    }
    
    .search-filters {
      display: flex;
      gap: 0.75rem;
    }
    
    button {
      cursor: pointer;
      font-weight: 500;
      transition: var(--transition);
      border: none;
      border-radius: var(--radius);
      padding: 0.5rem 1rem;
    }
    
    .btn-primary {
      background-color: var(--primary);
      color: white;
    }
    
    .btn-primary:hover {
      background-color: var(--primary-hover);
    }
    
    .btn-secondary {
      background-color: var(--border);
      color: var(--text);
    }
    
    .btn-secondary:hover {
      background-color: var(--border-dark);
    }
    
    .btn-danger {
      background-color: var(--danger);
      color: white;
    }
    
    .btn-danger:hover {
      background-color: var(--danger-hover);
    }
    
    .btn-cancel {
      background-color: var(--border);
      color: var(--text);
    }
    
    .btn-cancel:hover {
      background-color: var(--border-dark);
    }
    
    .btn-save {
      background-color: var(--primary);
      color: white;
    }
    
    .btn-save:hover {
      background-color: var(--primary-hover);
    }
    
    .btn-back {
      background-color: var(--border);
      color: var(--text);
    }
    
    .btn-back:hover {
      background-color: var(--border-dark);
    }
    
    .btn-clear-filters {
      background: none;
      color: var(--primary);
      text-decoration: underline;
      padding: 0;
    }
    
    input, select, textarea {
      width: 100%;
      padding: 0.5rem 0.75rem;
      border: 1px solid var(--border);
      border-radius: var(--radius);
      font-family: inherit;
      transition: var(--transition);
    }
    
    input:focus, select:focus, textarea:focus {
      outline: none;
      border-color: var(--primary);
      box-shadow: 0 0 0 3px var(--primary-light);
    }
    
    .search-input {
      width: 240px;
    }
    
    .department-select {
      width: 180px;
    }
    
    .class-grid {
      display: grid;
      gap: 1.5rem;
      grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
    }
    
    .class-card {
      background-color: var(--card-bg);
      border: 1px solid var(--border);
      border-radius: var(--radius);
      overflow: hidden;
      box-shadow: var(--shadow-sm);
      transition: var(--transition);
    }
    
    .class-card:hover {
      transform: translateY(-2px);
      box-shadow: var(--shadow-md);
      cursor: pointer;
    }
    
    .class-card-header {
      padding: 1.25rem;
      border-bottom: 1px solid var(--border);
      background-color: var(--primary-light);
    }
    
    .class-card-header h3 {
      font-weight: 600;
      margin-bottom: 0.25rem;
    }
    
    .class-card-header p {
      color: var(--text-light);
      font-size: 0.875rem;
    }
    
    .class-card-body {
      padding: 1.25rem;
    }
    
    .class-info-grid {
      display: grid;
      grid-template-columns: repeat(2, 1fr);
      gap: 0.75rem;
      font-size: 0.875rem;
    }
    
    .class-info-grid span {
      font-weight: 500;
      color: var(--text-light);
      display: block;
      margin-bottom: 0.25rem;
    }
    
    .schedule-text {
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
    }
    
    .no-results {
      grid-column: 1 / -1;
      padding: 3rem 1rem;
      text-align: center;
      background-color: var(--border);
      border-radius: var(--radius);
    }
    
    .no-results p {
      color: var(--text-light);
      font-size: 1.125rem;
      margin-bottom: 1rem;
    }
    
    .class-detail-container {
      background-color: var(--card-bg);
      border-radius: var(--radius);
      border: 1px solid var(--border);
      box-shadow: var(--shadow-sm);
      overflow: hidden;
    }
    
    .class-detail-header {
      padding: 1.5rem;
      border-bottom: 1px solid var(--border);
      display: flex;
      justify-content: space-between;
      align-items: center;
      background-color: var(--primary-light);
    }
    
    .class-detail-header h2 {
      font-size: 1.5rem;
      font-weight: 700;
    }
    
    .class-detail-header p {
      color: var(--text-light);
    }
    
    .detail-actions {
      display: flex;
      gap: 0.75rem;
    }
    
    .class-detail-body {
      padding: 1.5rem;
      display: grid;
      gap: 2rem;
      grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
    }
    
    .class-detail-body h3 {
      font-size: 1.125rem;
      font-weight: 600;
      margin-bottom: 1rem;
      color: var(--primary);
    }
    
    .detail-info-grid {
      display: grid;
      gap: 1rem;
    }
    
    .detail-info-grid span {
      font-weight: 500;
      color: var(--text-light);
    }
    
    .enrollment-bar {
      width: 100%;
      height: 0.5rem;
      background-color: var(--border);
      border-radius: 0.25rem;
      margin-top: 0.5rem;
      overflow: hidden;
    }
    
    .enrollment-progress {
      height: 100%;
      background-color: var(--primary);
      border-radius: 0.25rem;
    }
    
    .class-description {
      line-height: 1.7;
      margin-bottom: 1.5rem;
    }
    
    .action-buttons {
      display: flex;
      flex-wrap: wrap;
      gap: 0.5rem;
    }
    
    .class-detail-footer {
      padding: 1.5rem;
      border-top: 1px solid var(--border);
    }
    
    .form-container {
      background-color: var(--card-bg);
      border-radius: var(--radius);
      border: 1px solid var(--border);
      box-shadow: var(--shadow-sm);
      overflow: hidden;
      max-width: 800px;
      margin: 0 auto;
    }
    
    .form-header {
      padding: 1.5rem;
      border-bottom: 1px solid var(--border);
      background-color: var(--primary-light);
    }
    
    .form-header h2 {
      font-size: 1.25rem;
      font-weight: 600;
    }
    
    .class-form {
      padding: 1.5rem;
    }
    
    .form-grid {
      display: grid;
      gap: 1.5rem;
      grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
    }
    
    .form-group {
      margin-bottom: 1.25rem;
    }
    
    .form-group label {
      display: block;
      margin-bottom: 0.5rem;
      font-weight: 500;
    }
    
    .form-actions {
      display: flex;
      justify-content: flex-end;
      gap: 0.75rem;
      margin-top: 1.5rem;
    }
    
    .text-red-600 {
      color: var(--danger);
    }
    
    .text-amber-500 {
      color: var(--warning);
    }
    
    .text-green-600 {
      color: var(--success);
    }
    
    @media (max-width: 768px) {
      .school-management {
        padding: 1rem;
      }
      
      .main-nav {
        flex-direction: column;
        gap: 1rem;
        align-items: stretch;
      }
      
      .search-filters {
        flex-direction: column;
      }
      
      .search-input, .department-select {
        width: 100%;
      }
    }
  </style>