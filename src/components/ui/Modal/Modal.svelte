<script>
    import { createEventDispatcher } from 'svelte';
  
    export let isOpen = false;
    export let name = '';
    export let grade = '';
    export let section = '';
    export let attendance = 100;
    export let performance = 'Good';
  
    const dispatch = createEventDispatcher();
  
    function closeModal() {
      isOpen = false;
      resetForm();
    }
  
    function resetForm() {
      name = '';
      grade = '';
      section = '';
      attendance = 100;
      performance = 'Good';
    }
  
    function handleSubmit() {
      const student = { name, grade, section, attendance, performance };
      dispatch('addStudent', student);
      closeModal();
    }
  </script>
  
  {#if isOpen}
    <div class="modal-overlay" on:click={closeModal} on:keydown={closeModal}>
      <div class="modal-content" on:click|stopPropagation on:keydown|stopPropagation>
        <button class="close-button" on:click={closeModal}>&times;</button>
        <h3 class="modal-title">Add New Student</h3>
        <form on:submit|preventDefault={handleSubmit}>
          <label for="name">Name</label>
          <input type="text" id="name" bind:value={name} required>
  
          <label for="grade">Grade</label>
          <input type="text" id="grade" bind:value={grade} required>
  
          <label for="section">Section</label>
          <input type="text" id="section" bind:value={section} required>
  
          <label for="attendance">Attendance (%)</label>
          <input type="number" id="attendance" bind:value={attendance} min="0" max="100" required>
  
          <label for="performance">Performance</label>
          <select id="performance" bind:value={performance}>
            <option value="Excellent">Excellent</option>
            <option value="Good">Good</option>
            <option value="Average">Average</option>
            <option value="Poor">Poor</option>
          </select>
  
          <div class="button-group">
            <button type="submit" class="primary-button">Add Student</button>
            <button type="button" class="secondary-button" on:click={closeModal}>Cancel</button>
          </div>
        </form>
      </div>
    </div>
  {/if}
  
  <style>
    .modal-overlay {
      position: fixed;
      inset: 0;
      background: rgba(0, 0, 0, 0.5);
      display: flex;
      justify-content: center;
      align-items: center;
      animation: fadeIn 0.3s ease-in-out;
    }
  
    .modal-content {
      background: white;
      padding: 2rem;
      border-radius: 8px;
      width: 90%;
      max-width: 400px;
      box-shadow: 0px 10px 30px rgba(0, 0, 0, 0.3);
      animation: slideUp 0.3s ease-in-out;
      position: relative;
    }
  
    .close-button {
      position: absolute;
      top: 10px;
      right: 15px;
      background: none;
      border: none;
      font-size: 1.5rem;
      cursor: pointer;
    }
  
    .modal-title {
      text-align: center;
      margin-bottom: 1rem;
      font-size: 1.25rem;
      font-weight: bold;
    }
  
    label {
      display: block;
      font-weight: 600;
      margin-top: 0.5rem;
    }
  
    input, select {
      width: 100%;
      padding: 0.5rem;
      margin-top: 0.3rem;
      border: 1px solid #ccc;
      border-radius: 4px;
    }
  
    .button-group {
      display: flex;
      justify-content: space-between;
      margin-top: 1.5rem;
    }
  
    .primary-button {
      background: #007bff;
      color: white;
      padding: 0.5rem 1rem;
      border: none;
      border-radius: 4px;
      cursor: pointer;
    }
  
    .secondary-button {
      background: #6c757d;
      color: white;
      padding: 0.5rem 1rem;
      border: none;
      border-radius: 4px;
      cursor: pointer;
    }
  
    .primary-button:hover {
      background: #0056b3;
    }
  
    .secondary-button:hover {
      background: #5a6268;
    }
  
    @keyframes fadeIn {
      from { opacity: 0; }
      to { opacity: 1; }
    }
  
    @keyframes slideUp {
      from { transform: translateY(20px); opacity: 0; }
      to { transform: translateY(0); opacity: 1; }
    }
  </style>
  