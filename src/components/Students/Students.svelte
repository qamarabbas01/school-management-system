<script>
  import Button from '../ui/Button/Button.svelte';
  import Modal from '../ui/Modal/Modal.svelte';
  export let students = [];
  
  let searchTerm = '';
  let isModalOpen = false;
  let nextId = students.length ? Math.max(...students.map(s => s.id)) + 1 : 1;

  $: filteredStudents = students.filter(student => 
    student.name.toLowerCase().includes(searchTerm.toLowerCase()) ||
    student.grade.toLowerCase().includes(searchTerm.toLowerCase()) ||
    student.section.toLowerCase().includes(searchTerm.toLowerCase())
  );

  function openModal() {
    isModalOpen = true;
  }

  function addStudent(event) {
    const newStudent = { ...event.detail, id: nextId++ };
    students = [...students, newStudent];
  }
</script>

<div class="students-page">
  <h1>Students</h1>
  
  <div class="controls">
    <input
      type="text"
      placeholder="Search students..."
      bind:value={searchTerm}
    />
    <Button onClick={openModal} mode="success" className="add-btn">+ Add Student</Button>
  </div>
  
  <div class="students-table">
    <table>
      <thead>
        <tr>
          <th>ID</th>
          <th>Name</th>
          <th>Grade</th>
          <th>Section</th>
          <th>Attendance</th>
          <th>Performance</th>
          <th>Actions</th>
        </tr>
      </thead>
      <tbody>
        {#each filteredStudents as student}
          <tr>
            <td>{student.id}</td>
            <td>{student.name}</td>
            <td>{student.grade}</td>
            <td>{student.section}</td>
            <td>{student.attendance}%</td>
            <td>{student.performance}</td>
            <td class="actions">
              <Button mode="primary" class="view-btn">View</Button>
              <Button mode="warning" className="edit-btn">Edit</Button>
            </td>
          </tr>
        {/each}
      </tbody>
    </table>
  </div>
</div>

<Modal bind:isOpen={isModalOpen} on:addStudent={addStudent} />

<style>
  .students-page {
    padding: 20px;
  }
  
  h1 {
    margin-bottom: 20px;
    color: #2c3e50;
  }
  
  .controls {
    display: flex;
    justify-content: space-between;
    margin-bottom: 20px;
  }
  
  input {
    padding: 10px;
    border: 1px solid #ddd;
    border-radius: 4px;
    width: 300px;
  }
  
  .students-table {
    background-color: white;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    overflow: hidden;
  }
  
  table {
    width: 100%;
    border-collapse: collapse;
  }
  
  th, td {
    padding: 15px;
    text-align: left;
    border-bottom: 1px solid #ddd;
  }
  
  th {
    background-color: #f8f9fa;
    font-weight: bold;
  }
  
  .actions {
    display: flex;
    gap: 10px;
  }
  
</style>