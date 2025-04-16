<script>
    import { fade, scale, slide } from 'svelte/transition';
    import { onMount, onDestroy } from 'svelte';
    export let services = [
      { name: 'Health', icon: '🩺', description: 'Community health support and awareness.' },
      { name: 'Hospital', icon: '🏥', description: 'Emergency and regular hospital services with top care and 24/7 availability.' },
      { name: 'Security', icon: '🛡️', description: 'Law enforcement, patrols, emergency response, and crime prevention.' },
      { name: 'Labs', icon: '🔬', description: 'Cutting-edge medical and research laboratories with testing and diagnostics.' },
      { name: 'Courses', icon: '📚', description: 'Wide variety of educational programs, online and in-person.' },
      { name: 'Transport', icon: '🚌', description: 'City, intercity, and emergency transport services available.' },
      { name: 'Food', icon: '🍽️', description: 'Nutritious food support, meal distribution, and canteen services.' }
    ];
  
    let selected = null;
  
    const openModal = (service) => {
      selected = service;
      document.body.style.overflow = 'hidden'; // Lock scroll
    };
  
    const closeModal = () => {
      selected = null;
      document.body.style.overflow = ''; // Unlock scroll
    };
  
    const handleKeyDown = (e) => {
      if (e.key === 'Escape') closeModal();
    };
  
    onMount(() => {
      window.addEventListener('keydown', handleKeyDown);
    });
  
    onDestroy(() => {
      window.removeEventListener('keydown', handleKeyDown);
      document.body.style.overflow = '';
    });
  </script>
  
  <style>
    :global(body) {
      margin: 0;
      font-family: "Inter", sans-serif;
      background: linear-gradient(to right top, #0f2027, #203a43, #2c5364);
      color: #fff;
    }
  
    .grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
      gap: 2rem;
      padding: 3rem 2rem;
    }
  
    .card {
      background: rgba(255, 255, 255, 0.05);
      backdrop-filter: blur(10px);
      border-radius: 20px;
      padding: 2rem;
      text-align: center;
      border: 1px solid rgba(255, 255, 255, 0.1);
      transition: all 0.3s ease;
      box-shadow: 0 6px 20px rgba(0, 0, 0, 0.25);
      cursor: pointer;
    }
  
    .card:hover {
      transform: translateY(-10px) scale(1.02);
      box-shadow: 0 12px 30px rgba(0, 0, 0, 0.4);
      border-color: rgba(255, 255, 255, 0.3);
    }
  
    .icon {
      font-size: 3.5rem;
      margin-bottom: 1rem;
      filter: drop-shadow(0 0 5px rgba(255, 255, 255, 0.5));
    }
  
    .name {
      font-size: 1.5rem;
      font-weight: bold;
    }
  
    .description {
      margin-top: 0.5rem;
      color: #ccc;
      font-size: 0.95rem;
    }
  
    /* MODAL */
    .modal-backdrop {
      position: fixed;
      inset: 0;
      background: rgba(0, 0, 0, 0.7);
      backdrop-filter: blur(6px);
      z-index: 100;
      display: flex;
      align-items: center;
      justify-content: center;
    }
  
    .modal {
      background: linear-gradient(to right, #1f1c2c, #928dab);
      border-radius: 20px;
      padding: 2.5rem 2rem;
      max-width: 500px;
      width: 90%;
      color: white;
      position: relative;
      box-shadow: 0 20px 60px rgba(0, 0, 0, 0.4);
      border: 1px solid rgba(255, 255, 255, 0.2);
    }
  
    .modal .icon {
      font-size: 4rem;
      margin-bottom: 1rem;
      animation: pulse 2s infinite;
    }
  
    .modal .title {
      font-size: 2rem;
      margin-bottom: 0.5rem;
      font-weight: 700;
    }
  
    .modal .desc {
      color: #eee;
      font-size: 1rem;
      margin-bottom: 1.5rem;
    }
  
    .modal .btn {
      background: #ffffff10;
      padding: 0.6rem 1.2rem;
      border-radius: 12px;
      border: 1px solid #ffffff30;
      color: #fff;
      font-weight: bold;
      cursor: pointer;
      transition: background 0.2s ease;
    }
  
    .modal .btn:hover {
      background: #ffffff25;
    }
  
    .close {
      position: absolute;
      top: 1rem;
      right: 1.2rem;
      font-size: 1.5rem;
      background: transparent;
      border: none;
      color: #fff;
      cursor: pointer;
      transition: color 0.2s ease;
    }
  
    .close:hover {
      color: #ffaaaa;
    }
  
    @keyframes pulse {
      0%, 100% {
        filter: drop-shadow(0 0 4px rgba(255, 255, 255, 0.6));
      }
      50% {
        filter: drop-shadow(0 0 12px rgba(255, 255, 255, 0.9));
      }
    }
  </style>
  
  <div class="grid">
    {#each services as service}
      <button class="card" on:click={() => openModal(service)} on:keydown={(e) => e.key === 'Enter' && openModal(service)} aria-label={`Open details for ${service.name}`}>
        <div class="icon">{service.icon}</div>
        <div class="name">{service.name}</div>
        <div class="description">{service.description.slice(0, 60)}...</div>
      </button>
    {/each}
  </div>
  
  {#if selected}
    <div class="modal-backdrop" on:click|self={closeModal} on:keydown={(e) => e.key === 'Escape' && closeModal()} role="dialog" aria-label="Service details modal" transition:fade>
      <div class="modal" in:slide={{ y: 200 }} out:fade>
        <button class="close" on:click={closeModal}>✕</button>
        <div class="icon">{selected.icon}</div>
        <div class="title">{selected.name}</div>
        <div class="desc">{selected.description}</div>
        <button class="btn" on:click={() => alert(`Opening full details for ${selected.name}`)}>
          View More
        </button>
      </div>
    </div>
  {/if}
  