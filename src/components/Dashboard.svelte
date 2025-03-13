<script>
  import AttendanceChart from './AttendanceChart.svelte';
  export let schoolStats;

  let activities = [
    { id: 1, type: 'registration', description: 'New student registered', time: '2 hours ago' },
    { id: 2, type: 'attendance', description: 'Attendance marked for Class 10', time: '3 hours ago' },
    { id: 3, type: 'exam', description: 'Math exam results uploaded', time: '5 hours ago' },
    { id: 4, type: 'notice', description: 'New notice added about Sports Day', time: '1 day ago' },
    { id: 5, type: 'fee', description: 'Fee payment received from Jack Smith', time: '1 day ago' }
  ];

  let attendanceData = {
    daily: [],
    monthly: [],
    yearly: [] 
  };
</script>

<div class="dashboard">
  <h1>Dashboard</h1>

  <div class="stats-container">
    <div class="stat-card">
      <div class="stat-icon students">👨‍🎓</div>
      <div class="stat-info">
        <h3>Total Students</h3>
        <p class="stat-value">{schoolStats.totalStudents}</p>
      </div>
    </div>

    <div class="stat-card">
      <div class="stat-icon teachers">👩‍🏫</div>
      <div class="stat-info">
        <h3>Total Teachers</h3>
        <p class="stat-value">{schoolStats.totalTeachers}</p>
      </div>
    </div>

    <div class="stat-card">
      <div class="stat-icon classes">🏫</div>
      <div class="stat-info">
        <h3>Total Classes</h3>
        <p class="stat-value">{schoolStats.totalClasses}</p>
      </div>
    </div>

    <div class="stat-card">
      <div class="stat-icon attendance">📋</div>
      <div class="stat-info">
        <h3>Attendance Rate</h3>
        <p class="stat-value">{schoolStats.attendance}%</p>
      </div>
    </div>
  </div>

  <div class="dashboard-content">
    <div class="chart-container">
      <AttendanceChart {attendanceData} />
    </div>

    <div class="activities-container">
      <h2>Recent Activities</h2>
      <ul class="activities-list">
        {#each activities as activity}
          <li class="activity-item">
            <div class="activity-icon">
              {#if activity.type === 'registration'}
                👤
              {:else if activity.type === 'attendance'}
                📋
              {:else if activity.type === 'exam'}
                📝
              {:else if activity.type === 'notice'}
                📢
              {:else if activity.type === 'fee'}
                💰
              {/if}
            </div>
            <div class="activity-details">
              <p class="activity-description">{activity.description}</p>
              <p class="activity-time">{activity.time}</p>
            </div>
          </li>
        {/each}
      </ul>
    </div>
  </div>
</div>

<style>
  .dashboard {
    padding: 20px;
    background-color: #f5f7fa;
    border-radius: 8px;
  }

  h1 {
    margin-bottom: 20px;
    color: #2c3e50;
  }

  .stats-container {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
    gap: 20px;
    margin-bottom: 30px;
  }

  .stat-card {
    background-color: white;
    border-radius: 8px;
    padding: 20px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    display: flex;
    align-items: center;
    transition: transform 0.2s;
  }

  .stat-card:hover {
    transform: translateY(-5px);
  }

  .stat-icon {
    font-size: 40px;
    margin-right: 20px;
  }

  .stat-info h3 {
    margin: 0;
    color: #34495e;
  }

  .stat-value {
    font-size: 24px;
    font-weight: bold;
    color: #2c3e50;
  }

  .dashboard-content {
    display: flex;
    gap: 20px;
  }

  .chart-container,
  .activities-container {
    flex: 1;
    background-color: white;
    border-radius: 8px;
    padding: 20px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  }

  .activities-container h2 {
    margin-top: 0;
    color: #2c3e50;
  }

  .activities-list {
    list-style: none;
    padding: 0;
    margin: 0;
  }

  .activity-item {
    display: flex;
    align-items: center;
    padding: 10px 0;
    border-bottom: 1px solid #ecf0f1;
  }

  .activity-item:last-child {
    border-bottom: none;
  }

  .activity-icon {
    font-size: 24px;
    margin-right: 10px;
  }

  .activity-details {
    flex: 1;
  }

  .activity-description {
    margin: 0;
    color: #34495e;
  }

  .activity-time {
    margin: 0;
    color: #7f8c8d;
    font-size: 12px;
  }
</style>
