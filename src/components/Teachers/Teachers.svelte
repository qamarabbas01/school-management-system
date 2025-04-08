<script>
    import { fade } from "svelte/transition";
  
    const teachers = [
      {
        name: "Dr. Sarah Johnson",
        subject: "Mathematics",
        role: "Department Head",
        image: "https://randomuser.me/api/portraits/women/1.jpg",
        exper: "15 years of experience",
      },
      {
        name: "Prof. Michael Chen",
        subject: "Physics",
        role: "Senior Lecturer",
        image: "https://randomuser.me/api/portraits/men/2.jpg",
        exper: "12 years of experience",
      },
      {
        name: "Dr. Emily Rodriguez",
        subject: "Biology",
        role: "Research Coordinator",
        image: "https://randomuser.me/api/portraits/women/3.jpg",
        exper: "10 years of experience",
      },
      {
        name: "James Wilson",
        subject: "Chemistry",
        role: "Laboratory Supervisor",
        image: "https://randomuser.me/api/portraits/men/4.jpg",
        exper: "8 years of experience",
      },
      {
        name: "Dr. Aisha Patel",
        subject: "Computer Science",
        role: "Program Director",
        image: "https://randomuser.me/api/portraits/women/5.jpg",
        exper: "14 years of experience",
      },
      {
        name: "Robert Thompson",
        subject: "History",
        role: "Associate Professor",
        image: "https://randomuser.me/api/portraits/men/6.jpg",
        exper: "9 years of experience",
      },
      {
        name: "Dr. Lisa Martinez",
        subject: "Psychology",
        role: "Student Advisor",
        image: "https://randomuser.me/api/portraits/women/7.jpg",
        exper: "11 years of experience",
      },
      {
        name: "Prof. David Kim",
        subject: "English Literature",
        role: "Department Coordinator",
        image: "https://randomuser.me/api/portraits/men/8.jpg",
        exper: "16 years of experience",
      },
      {
        name: "Dr. Olivia Taylor",
        subject: "Sociology",
        role: "Research Fellow",
        image: "https://randomuser.me/api/portraits/women/9.jpg",
        exper: "7 years of experience",
      },
      {
        name: "Thomas Jackson",
        subject: "Art & Design",
        role: "Studio Director",
        image: "https://randomuser.me/api/portraits/men/10.jpg",
        exper: "13 years of experience",
      },
      {
        name: "Dr. Grace Lee",
        subject: "Economics",
        role: "Graduate Coordinator",
        image: "https://randomuser.me/api/portraits/women/11.jpg",
        exper: "9 years of experience",
      },
      {
        name: "Prof. Samuel Brown",
        subject: "Philosophy",
        role: "Department Chair",
        image: "https://randomuser.me/api/portraits/men/12.jpg",
        exper: "18 years of experience",
      },
      {
        name: "Dr. Natalie Wright",
        subject: "Linguistics",
        role: "Research Director",
        image: "https://randomuser.me/api/portraits/women/15.jpg",
        exper: "13 years of experience",
      },
      {
        name: "Prof. Marcus Alvarez",
        subject: "Music Theory",
        role: "Performance Coordinator",
        image: "https://randomuser.me/api/portraits/men/15.jpg",
        exper: "20 years of experience",
      },
      {
        name: "Dr. Jennifer Kang",
        subject: "Environmental Science",
        role: "Field Study Supervisor",
        image: "https://randomuser.me/api/portraits/women/20.jpg",
        exper: "11 years of experience",
      },
    ];
  
    let searchQuery = "";
  
    $: filteredTeachers = teachers.filter((teacher) => {
      if (!searchQuery.trim()) return true;
      const query = searchQuery.toLowerCase();
      return (
        teacher.name.toLowerCase().includes(query) ||
        teacher.subject.toLowerCase().includes(query) ||
        teacher.role.toLowerCase().includes(query)
      );
    });
  
    function handleImageError(event) {
      event.target.src = "https://via.placeholder.com/300x300?text=Teacher";
    }
  </script>
  
  <div class="container">
    <h1 class="title">Our Faculty</h1>
    <p class="subtitle">Meet our experienced teaching staff</p>
  
    <div class="search-box">
      <input
        type="text"
        bind:value={searchQuery}
        placeholder="Search teachers..."
        class="search-input"
      />
    </div>
  
    <div class="teacher-grid">
      {#each filteredTeachers as teacher, i}
        <div in:fade={{ duration: 200, delay: i * 50 }} class="card">
          <div class="card-image">
            <img
              src={teacher.image || "/placeholder.svg"}
              alt={teacher.name}
              on:error={handleImageError}
              class="card-img"
            />
            <div class="card-overlay">
              <span class="experience">{teacher.exper}</span>
            </div>
          </div>
  
          <div class="card-content">
            <h2 class="card-title">{teacher.name}</h2>
            <p class="card-subtitle">{teacher.subject}</p>
            <div class="card-footer">
              <span class="card-role">{teacher.role}</span>
              <svg
                class="card-icon"
                fill="none"
                stroke="currentColor"
                viewBox="0 0 24 24"
              >
                <path
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  stroke-width="2"
                  d="M5 12h.01M12 12h.01M19 12h.01M6 12a1 1 0 11-2 0 1 1 0 012 0zm7 0a1 1 0 11-2 0 1 1 0 012 0zm7 0a1 1 0 11-2 0 1 1 0 012 0z"
                ></path>
              </svg>
            </div>
          </div>
        </div>
      {/each}
    </div>
  
    {#if filteredTeachers.length === 0}
      <div class="no-results">
        <p>No teachers found matching your search.</p>
      </div>
    {/if}
  </div>
  
  <style>
    .container {
      margin: auto;
      padding: 20px;
      text-align: center;
    }
  
    .title {
      font-size: 2rem;
      font-weight: bold;
      color: #333;
    }
  
    .subtitle {
      color: #666;
      margin-bottom: 20px;
    }
  
    .search-box {
      margin: 20px auto;
      max-width: 400px;
    }
  
    .search-input {
      width: 100%;
      padding: 10px;
      border: 1px solid #ccc;
      border-radius: 5px;
      font-size: 16px;
    }
  
    .teacher-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
      gap: 20px;
      padding: 20px;
    }
  
    .card {
      background-color: white;
      border-radius: 12px;
      box-shadow: 0 2px 6px rgba(0, 0, 0, 0.1);
      overflow: hidden;
      transition: box-shadow 0.3s ease;
    }
  
    .card:hover {
      box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
    }
  
    .card-image {
      position: relative;
      height: 192px; /* 48 * 4 (rem) */
      overflow: hidden;
    }
  
    .card-img {
      width: 100%;
      height: 100%;
      object-fit: cover;
      transition: transform 0.5s ease;
    }
  
    .card-img:hover {
      transform: scale(1.05);
    }
  
    .card-overlay {
      position: absolute;
      bottom: 0;
      left: 0;
      right: 0;
      background: linear-gradient(to top, rgba(0, 0, 0, 0.6), transparent);
      padding: 12px;
    }
  
    .experience {
      color: white;
      font-size: 14px;
      font-weight: 500;
    }
  
    .card-content {
      padding: 16px;
    }
  
    .card-title {
      font-size: 18px;
      font-weight: bold;
      color: #2d3748;
    }
  
    .card-subtitle {
      color: #2563eb;
      font-weight: 500;
    }
  
    .card-footer {
      margin-top: 8px;
      display: flex;
      align-items: center;
      justify-content: space-between;
    }
  
    .card-role {
      background-color: #f3f4f6;
      color: #2d3748;
      font-size: 12px;
      font-weight: 500;
      padding: 4px 8px;
      border-radius: 4px;
    }
  
    .card-icon {
      width: 20px;
      height: 20px;
      color: #9ca3af;
    }
  
    .no-results {
      margin-top: 20px;
      color: #888;
    }
  </style>