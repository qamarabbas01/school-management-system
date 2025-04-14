<script>
    let events = [
      {
        title: "Annual Day Celebration",
        date: "April 20, 2025",
        location: "Auditorium",
        type: "Cultural"
      },
      {
        title: "Science Exhibition",
        date: "May 2, 2025",
        location: "Hall B",
        type: "Academic"
      },
      {
        title: "Inter-college Sports Meet",
        date: "May 15, 2025",
        location: "Main Ground",
        type: "Sports"
      },
      {
        title: "Tech Fest 2025",
        date: "June 5, 2025",
        location: "Innovation Lab",
        type: "Technology"
      }
    ];
  
    const typeColors = {
      Cultural: "#fce4ec",
      Academic: "#e3f2fd",
      Sports: "#e8f5e9",
      Technology: "#ede7f6"
    };
  
    const typeEmojis = {
      Cultural: "🎭",
      Academic: "📚",
      Sports: "🏅",
      Technology: "💻"
    };
  </script>
  
  <style>
    .wrapper {
      max-width: 700px;
      margin: 2rem auto;
      padding: 1rem;
      background: linear-gradient(to bottom right, #f3f4f6, #e0f7fa);
      border-radius: 1rem;
      box-shadow: 0 4px 20px rgba(0, 0, 0, 0.05);
    }
  
    h2 {
      text-align: center;
      font-size: 1.8rem;
      margin-bottom: 1.5rem;
      color: #333;
    }
  
    .event {
      background: #fff;
      border-radius: 12px;
      padding: 1.2rem 1.5rem;
      margin-bottom: 1rem;
      box-shadow: 0 4px 12px rgba(0, 0, 0, 0.06);
      transition: transform 0.25s ease, box-shadow 0.25s ease;
      cursor: pointer;
    }
  
    .event:hover {
      transform: translateY(-5px);
      box-shadow: 0 8px 20px rgba(0, 0, 0, 0.1);
    }
  
    .event-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
    }
  
    .event-title {
      font-size: 1.2rem;
      font-weight: bold;
      color: #222;
    }
  
    .event-tag {
      padding: 0.4rem 0.8rem;
      border-radius: 20px;
      font-size: 0.75rem;
      font-weight: 500;
      color: #444;
    }
  
    .event-info {
      margin-top: 0.6rem;
      font-size: 0.95rem;
      color: #555;
    }
  
    @media (max-width: 500px) {
      .event-title {
        font-size: 1rem;
      }
  
      .event-info {
        font-size: 0.85rem;
      }
    }
  </style>
  
  <div class="wrapper">
    <h2>📅 Upcoming Events</h2>
  
    {#each events as event}
      <div class="event">
        <div class="event-header">
          <div class="event-title">
            {typeEmojis[event.type] || "🎉"} {event.title}
          </div>
          <div
            class="event-tag"
            style="background-color: {typeColors[event.type] || '#eee'}"
          >
            {event.type}
          </div>
        </div>
        <div class="event-info">📍 {event.location}</div>
        <div class="event-info">🗓️ {event.date}</div>
      </div>
    {/each}
  </div>
  