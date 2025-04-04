<script>
	import { onMount } from 'svelte';
	import Dashboard from './components/Dashboard.svelte';
	import Sidebar from './components/Sidebar.svelte';
	import Header from './components/Header.svelte';
    import Students from './components/Students/Students.svelte';
  	import Teachers from './components/Teachers/Teachers.svelte';
  import Classes from './components/Classes/Classes.svelte';
	
	let currentView = 'dashboard';
	let user = { name: 'Admin User', role: 'Administrator' };
	
	let schoolStats = {
	  totalStudents: 0,
	  totalTeachers: 0,
	  totalClasses: 0,
	  attendance: 0,
	  leaveStudents: 0
	};
	
	let students = [];
	let loading = true;
	
	onMount(async () => {
	  await new Promise(resolve => setTimeout(resolve, 1000));

	  schoolStats = {
		totalStudents: 1250,
		totalTeachers: 75,
		totalClasses: 45,
		attendance: 92,
		leaveStudents: 25
	  };
	  
	  students = [
		{ id: 1, name: 'John Doe', grade: '10th', section: 'A', attendance: 95, performance: 'Excellent' },
		{ id: 2, name: 'Jane Smith', grade: '10th', section: 'B', attendance: 88, performance: 'Good' },
		{ id: 3, name: 'Michael Johnson', grade: '11th', section: 'A', attendance: 92, performance: 'Very Good' },
		{ id: 4, name: 'Sarah Williams', grade: '9th', section: 'C', attendance: 85, performance: 'Average' },
		{ id: 5, name: 'David Brown', grade: '12th', section: 'A', attendance: 98, performance: 'Excellent' },
		{ id: 6, name: 'Emily Davis', grade: '11th', section: 'B', attendance: 91, performance: 'Good' },
		{ id: 7, name: 'Robert Wilson', grade: '9th', section: 'A', attendance: 87, performance: 'Average' },
		{ id: 8, name: 'Jennifer Taylor', grade: '10th', section: 'C', attendance: 94, performance: 'Very Good' }
	  ];
	  
	  loading = false;
	});
	
	function handleNavigation(view) {
	  currentView = view;
	}
  </script>
  
  <main class="app">
	<Sidebar {currentView} {handleNavigation} />
	
	<div class="content">
	  <Header {user} />
	  
	  {#if loading}
		<div class="loading">Loading...</div>
	  {:else}
		{#if currentView === 'dashboard'}
		  <Dashboard {schoolStats} />
		{:else if currentView === 'students'}
		  <Students {students} />
		{:else if currentView === 'teachers'}
		  <Teachers/>
		{:else if currentView === 'classes'}
		   <Classes />
		{:else}
		  <div class="loading">Comming Soon...</div>
		{/if}
	  {/if}
	</div>
  </main>
  
  <style>
	.app {
	  display: flex;
	  height: 100vh;
	}
	
	.content {
	  flex: 1;
	  padding: 20px;
	  background-color: #f5f5f5;
	  overflow-y: auto;
	}
	
	.loading {
	  display: flex;
	  justify-content: center;
	  align-items: center;
	  height: 80vh;
	  font-size: 18px;
	}
  </style>