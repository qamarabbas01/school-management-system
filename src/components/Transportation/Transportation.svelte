<script>
    import { onMount } from 'svelte';

    let transportationOptions = [
      { id: 1, route: "Route A", destination: "North Campus", schedule: "7:30 AM, 3:30 PM", cost: "$2.50", status: "On Time" },
      { id: 2, route: "Route B", destination: "South Campus", schedule: "7:45 AM, 3:45 PM", cost: "$2.50", status: "On Time" },
      { id: 3, route: "Route C", destination: "East Campus", schedule: "8:00 AM, 4:00 PM", cost: "$3.00", status: "Delayed (10 min)" },
      { id: 4, route: "Route D", destination: "West Campus", schedule: "7:15 AM, 3:15 PM", cost: "$2.50", status: "On Time" },
      { id: 5, route: "Route E", destination: "Downtown", schedule: "7:00 AM, 3:00 PM", cost: "$3.50", status: "On Time" },
      { id: 6, route: "Route F", destination: "University Heights", schedule: "7:30 AM, 3:30 PM", cost: "$3.00", status: "Cancelled" }
    ];
    
    let searchQuery = "";
    let filteredOptions = transportationOptions;
  
    $: filteredOptions = transportationOptions.filter(option => 
      option.route.toLowerCase().includes(searchQuery.toLowerCase()) || 
      option.destination.toLowerCase().includes(searchQuery.toLowerCase())
    );
    
    let updates = [
      { id: 1, time: "8:15 AM", message: "Route C delayed by 10 minutes due to traffic" },
      { id: 2, time: "7:45 AM", message: "Route F cancelled today due to road construction" }
    ];
    
    let isMenuOpen = false;
    
    let name = "";
    let email = "";
    let message = "";
    
    function handleSubmit() {
      alert("Contact form submitted! (This would connect to a backend in a real application)");
      name = "";
      email = "";
      message = "";
    }
    
    let currentTime = new Date().toLocaleTimeString();
    
    onMount(() => {
      const interval = setInterval(() => {
        currentTime = new Date().toLocaleTimeString();
      }, 1000);
      
      return () => clearInterval(interval);
    });
  </script>
  
  <main>
    <header>
      <div class="logo">
        <h1>School Transport</h1>
      </div>
      <button class="menu-toggle" on:click={() => isMenuOpen = !isMenuOpen} aria-label="Toggle menu">
        <span></span>
        <span></span>
        <span></span>
      </button>
      <nav class:active={isMenuOpen}>
        <ul>
          <li><a href="#routes">Routes</a></li>
          <li><a href="#updates">Updates</a></li>
          <li><a href="#contact">Contact</a></li>
        </ul>
      </nav>
    </header>
    
    <section class="hero">
      <div class="container">
        <h2>School Transportation Services</h2>
        <p>Find routes, schedules, and real-time updates for all school transportation options.</p>
        <p class="current-time">Current time: {currentTime}</p>
      </div>
    </section>
    
    <section id="routes" class="routes">
      <div class="container">
        <h2>Transportation Routes</h2>
        
        <div class="search-container">
          <input 
            type="text" 
            placeholder="Search routes or destinations..." 
            bind:value={searchQuery}
            aria-label="Search routes or destinations"
          />
        </div>
        
        {#if filteredOptions.length === 0}
          <div class="no-results">
            <p>No routes found matching "{searchQuery}"</p>
          </div>
        {:else}
          <div class="route-cards">
            {#each filteredOptions as option}
              <div class="route-card" class:delayed={option.status.includes("Delayed")} class:cancelled={option.status.includes("Cancelled")}>
                <h3>{option.route}</h3>
                <div class="route-details">
                  <p><strong>Destination:</strong> {option.destination}</p>
                  <p><strong>Schedule:</strong> {option.schedule}</p>
                  <p><strong>Cost:</strong> {option.cost}</p>
                  <p class="status"><strong>Status:</strong> <span>{option.status}</span></p>
                </div>
              </div>
            {/each}
          </div>
        {/if}
      </div>
    </section>
    
    <section id="updates" class="updates">
      <div class="container">
        <h2>Real-time Updates</h2>
        <div class="updates-container">
          {#each updates as update}
            <div class="update-item">
              <span class="update-time">{update.time}</span>
              <p class="update-message">{update.message}</p>
            </div>
          {/each}
        </div>
      </div>
    </section>
    
    <section id="contact" class="contact">
      <div class="container">
        <h2>Contact Transportation Department</h2>
        <form on:submit|preventDefault={handleSubmit}>
          <div class="form-group">
            <label for="name">Name</label>
            <input type="text" id="name" bind:value={name} required />
          </div>
          <div class="form-group">
            <label for="email">Email</label>
            <input type="email" id="email" bind:value={email} required />
          </div>
          <div class="form-group">
            <label for="message">Message</label>
            <textarea id="message" bind:value={message} rows="5" required></textarea>
          </div>
          <button type="submit" class="submit-btn">Send Message</button>
        </form>
      </div>
    </section>
  </main>
  
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }
    
    .container {
      width: 90%;
      max-width: 1200px;
      margin: 0 auto;
      padding: 0 20px;
    }
    
    header {
      background-color: #4a5568;
      color: white;
      padding: 1rem 0;
      position: sticky;
      top: 0;
      z-index: 100;
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 1rem 2rem;
    }
    
    .logo h1 {
      font-size: 1.5rem;
      font-weight: 700;
    }
    
    nav ul {
      display: flex;
      list-style: none;
    }
    
    nav ul li {
      margin-left: 2rem;
    }
    
    nav ul li a {
      color: white;
      text-decoration: none;
      font-weight: 500;
      transition: color 0.3s;
    }
    
    nav ul li a:hover {
      color: #e2e8f0;
    }
    
    .menu-toggle {
      display: none;
      background: none;
      border: none;
      cursor: pointer;
    }
    
    .menu-toggle span {
      display: block;
      width: 25px;
      height: 3px;
      margin: 5px 0;
      background-color: white;
      transition: all 0.3s;
    }
    
    /* Hero section */
    .hero {
      background-color: #f7fafc;
      padding: 4rem 0;
      text-align: center;
    }
    
    .hero h2 {
      font-size: 2.5rem;
      margin-bottom: 1rem;
      color: #2d3748;
    }
    
    .hero p {
      font-size: 1.2rem;
      color: #4a5568;
      max-width: 700px;
      margin: 0 auto;
    }
    
    .current-time {
      margin-top: 1rem;
      font-weight: 600;
      color: #718096;
    }
    
    .routes {
      padding: 4rem 0;
      background-color: white;
    }
    
    .routes h2 {
      text-align: center;
      margin-bottom: 2rem;
      color: #2d3748;
    }
    
    .search-container {
      max-width: 600px;
      margin: 0 auto 2rem;
    }
    
    .search-container input {
      width: 100%;
      padding: 0.8rem 1rem;
      border: 2px solid #e2e8f0;
      border-radius: 5px;
      font-size: 1rem;
      transition: border-color 0.3s;
    }
    
    .search-container input:focus {
      outline: none;
      border-color: #4a5568;
    }
    
    .route-cards {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
      gap: 1.5rem;
    }
    
    .route-card {
      background-color: white;
      border-radius: 8px;
      box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
      padding: 1.5rem;
      transition: transform 0.3s, box-shadow 0.3s;
      border-top: 5px solid #4a5568;
    }
    
    .route-card:hover {
      transform: translateY(-5px);
      box-shadow: 0 10px 15px rgba(0, 0, 0, 0.1);
    }
    
    .route-card h3 {
      margin-bottom: 1rem;
      color: #2d3748;
      font-size: 1.3rem;
    }
    
    .route-details p {
      margin-bottom: 0.5rem;
    }
    
    .status span {
      font-weight: 600;
    }
    
    .delayed {
      border-top-color: #ed8936;
    }
    
    .delayed .status span {
      color: #ed8936;
    }
    
    .cancelled {
      border-top-color: #e53e3e;
    }
    
    .cancelled .status span {
      color: #e53e3e;
    }
    
    .no-results {
      text-align: center;
      padding: 2rem;
      background-color: #f7fafc;
      border-radius: 8px;
    }

    .updates {
      padding: 4rem 0;
      background-color: #f7fafc;
    }
    
    .updates h2 {
      text-align: center;
      margin-bottom: 2rem;
      color: #2d3748;
    }
    
    .updates-container {
      max-width: 800px;
      margin: 0 auto;
    }
    
    .update-item {
      background-color: white;
      border-radius: 8px;
      box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
      padding: 1.5rem;
      margin-bottom: 1rem;
      display: flex;
      align-items: flex-start;
    }
    
    .update-time {
      background-color: #4a5568;
      color: white;
      padding: 0.3rem 0.6rem;
      border-radius: 4px;
      font-size: 0.9rem;
      margin-right: 1rem;
      white-space: nowrap;
    }
    
    .update-message {
      margin: 0;
    }
    
    .contact {
      padding: 4rem 0;
      background-color: white;
    }
    
    .contact h2 {
      text-align: center;
      margin-bottom: 2rem;
      color: #2d3748;
    }
    
    form {
      max-width: 600px;
      margin: 0 auto;
    }
    
    .form-group {
      margin-bottom: 1.5rem;
    }
    
    label {
      display: block;
      margin-bottom: 0.5rem;
      font-weight: 500;
      color: #4a5568;
    }
    
    input, textarea {
      width: 100%;
      padding: 0.8rem;
      border: 2px solid #e2e8f0;
      border-radius: 5px;
      font-size: 1rem;
      transition: border-color 0.3s;
    }
    
    input:focus, textarea:focus {
      outline: none;
      border-color: #4a5568;
    }
    
    .submit-btn {
      background-color: #4a5568;
      color: white;
      border: none;
      padding: 0.8rem 1.5rem;
      border-radius: 5px;
      font-size: 1rem;
      cursor: pointer;
      transition: background-color 0.3s;
    }
    
    .submit-btn:hover {
      background-color: #2d3748;
    }
    
    @media (max-width: 768px) {
      .menu-toggle {
        display: block;
      }
      
      nav {
        position: fixed;
        top: 0;
        right: -100%;
        width: 70%;
        height: 100vh;
        background-color: #2d3748;
        padding-top: 5rem;
        transition: right 0.3s ease;
      }
      
      nav.active {
        right: 0;
      }
      
      nav ul {
        flex-direction: column;
      }
      
      nav ul li {
        margin: 1rem 0;
        margin-left: 2rem;
      }
      
      .hero h2 {
        font-size: 2rem;
      }
      
      .update-item {
        flex-direction: column;
      }
      
      .update-time {
        margin-bottom: 0.5rem;
        margin-right: 0;
      }
    }
    
    @media (max-width: 480px) {
      .route-cards {
        grid-template-columns: 1fr;
      }
      
      .hero h2 {
        font-size: 1.8rem;
      }
      
      .hero p {
        font-size: 1rem;
      }
    }
  </style>