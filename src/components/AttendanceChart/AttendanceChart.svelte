
<script>
  import { onMount } from 'svelte';
  
  export const schoolStats = {};
  
  let activeTab = 'daily';
  let selectedYear = new Date().getFullYear();
  let selectedMonth = new Date().getMonth();
  
  const monthNames = [
    'January', 'February', 'March', 'April', 'May', 'June',
    'July', 'August', 'September', 'October', 'November', 'December'
  ];
  
  const years = [
    selectedYear - 2, 
    selectedYear - 1, 
    selectedYear, 
    selectedYear + 1
  ];
  
  const dailyData = [
    { day: 'Monday', attendance: 94 },
    { day: 'Tuesday', attendance: 92 },
    { day: 'Wednesday', attendance: 89 },
    { day: 'Thursday', attendance: 93 },
    { day: 'Friday', attendance: 91 }
  ];
  
  const monthlyData = [
    { month: 'Jan', attendance: 91 },
    { month: 'Feb', attendance: 89 },
    { month: 'Mar', attendance: 93 },
    { month: 'Apr', attendance: 95 },
    { month: 'May', attendance: 92 },
    { month: 'Jun', attendance: 90 },
    { month: 'Jul', attendance: 87 },
    { month: 'Aug', attendance: 89 },
    { month: 'Sep', attendance: 91 },
    { month: 'Oct', attendance: 94 },
    { month: 'Nov', attendance: 93 },
    { month: 'Dec', attendance: 88 }
  ];
  
  const yearlyData = [
    { year: selectedYear - 4, attendance: 87 },
    { year: selectedYear - 3, attendance: 89 },
    { year: selectedYear - 2, attendance: 90 },
    { year: selectedYear - 1, attendance: 92 },
    { year: selectedYear, attendance: 93 }
  ];
  
  let daysInMonth = new Date(selectedYear, selectedMonth + 1, 0).getDate();
  let currentMonthData = [];
  
  function generateMonthDailyData() {
    const days = [];
    for (let i = 1; i <= daysInMonth; i++) {
      const dayOfWeek = new Date(selectedYear, selectedMonth, i).getDay();
      if (dayOfWeek !== 0 && dayOfWeek !== 6) {
        const attendance = Math.floor(Math.random() * 18) + 80;
        days.push({ day: i, attendance });
      }
    }
    return days;
  }
  
  $: {
    daysInMonth = new Date(selectedYear, selectedMonth + 1, 0).getDate();
    currentMonthData = generateMonthDailyData();
  }
  
  function setActiveTab(tab) {
    activeTab = tab;
  }
  
  function updateMonth(event) {
    selectedMonth = parseInt(event.target.value);
  }
  
  function updateYear(event) {
    selectedYear = parseInt(event.target.value);
  }
</script>

<div class="chart-container">
  <div class="chart-header">
    <h2>Attendance Overview</h2>
    <div class="chart-controls">
      <div class="tabs">
        <button 
          class={activeTab === 'daily' ? 'active' : ''}
          on:click={() => setActiveTab('daily')}
        >
          Daily
        </button>
        <button 
          class={activeTab === 'month' ? 'active' : ''}
          on:click={() => setActiveTab('month')}
        >
          Monthly
        </button>
        <button 
          class={activeTab === 'year' ? 'active' : ''}
          on:click={() => setActiveTab('year')}
        >
          Yearly
        </button>
      </div>
      
      {#if activeTab === 'daily'}
        <div class="date-selectors">
          <select on:change={updateMonth}>
            {#each monthNames as month, index}
              <option value={index} selected={index === selectedMonth}>
                {month}
              </option>
            {/each}
          </select>
          
          <select on:change={updateYear}>
            {#each years as year}
              <option value={year} selected={year === selectedYear}>
                {year}
              </option>
            {/each}
          </select>
        </div>
      {/if}
    </div>
  </div>
  
  <div class="chart-content">
    {#if activeTab === 'daily'}
      <div class="attendance-chart">
        <div class="chart-bars">
          {#each currentMonthData as data}
            <div class="chart-bar-container">
              <div class="chart-bar" style="height: {data.attendance}%"></div>
              <div class="chart-label">{data.day}</div>
            </div>
          {/each}
        </div>
        <div class="chart-axis">
          <div>100%</div>
          <div>75%</div>
          <div>50%</div>
          <div>25%</div>
          <div>0%</div>
        </div>
      </div>
      <div class="chart-summary">
        <div class="summary-item">
          <span class="summary-label">Month Average:</span>
          <span class="summary-value">
            {Math.round(currentMonthData.reduce((sum, d) => sum + d.attendance, 0) / currentMonthData.length)}%
          </span>
        </div>
        <div class="summary-item">
          <span class="summary-label">Highest Day:</span>
          <span class="summary-value">
            {currentMonthData.length > 0 ? Math.max(...currentMonthData.map(d => d.attendance)) : 0}%
          </span>
        </div>
        <div class="summary-item">
          <span class="summary-label">Lowest Day:</span>
          <span class="summary-value">
            {currentMonthData.length > 0 ? Math.min(...currentMonthData.map(d => d.attendance)) : 0}%
          </span>
        </div>
      </div>
    {:else if activeTab === 'month'}
      <div class="attendance-chart">
        <div class="chart-bars">
          {#each monthlyData as data}
            <div class="chart-bar-container">
              <div class="chart-bar" style="height: {data.attendance}%"></div>
              <div class="chart-label">{data.month}</div>
            </div>
          {/each}
        </div>
        <div class="chart-axis">
          <div>100%</div>
          <div>75%</div>
          <div>50%</div>
          <div>25%</div>
          <div>0%</div>
        </div>
      </div>
      <div class="chart-summary">
        <div class="summary-item">
          <span class="summary-label">Year Average:</span>
          <span class="summary-value">
            {Math.round(monthlyData.reduce((sum, d) => sum + d.attendance, 0) / monthlyData.length)}%
          </span>
        </div>
        <div class="summary-item">
          <span class="summary-label">Highest Month:</span>
          <span class="summary-value">
            {Math.max(...monthlyData.map(d => d.attendance))}%
          </span>
        </div>
        <div class="summary-item">
          <span class="summary-label">Lowest Month:</span>
          <span class="summary-value">
            {Math.min(...monthlyData.map(d => d.attendance))}%
          </span>
        </div>
      </div>
    {:else if activeTab === 'year'}
      <div class="attendance-chart">
        <div class="chart-bars">
          {#each yearlyData as data}
            <div class="chart-bar-container">
              <div class="chart-bar" style="height: {data.attendance}%"></div>
              <div class="chart-label">{data.year}</div>
            </div>
          {/each}
        </div>
        <div class="chart-axis">
          <div>100%</div>
          <div>75%</div>
          <div>50%</div>
          <div>25%</div>
          <div>0%</div>
        </div>
      </div>
      <div class="chart-summary">
        <div class="summary-item">
          <span class="summary-label">5-Year Trend:</span>
          <span class="summary-value">
            {yearlyData[yearlyData.length - 1].attendance > yearlyData[0].attendance ? 'Increasing' : 'Decreasing'}
          </span>
        </div>
        <div class="summary-item">
          <span class="summary-label">Overall Average:</span>
          <span class="summary-value">
            {Math.round(yearlyData.reduce((sum, d) => sum + d.attendance, 0) / yearlyData.length)}%
          </span>
        </div>
        <div class="summary-item">
          <span class="summary-label">Current Year:</span>
          <span class="summary-value">
            {yearlyData[yearlyData.length - 1].attendance}%
          </span>
        </div>
      </div>
    {/if}
  </div>
</div>

<style>
  .chart-container {
    background-color: white;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    padding: 20px;
    margin-bottom: 20px;
  }
  
  .chart-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 20px;
  }
  
  h2 {
    margin: 0;
    color: #2c3e50;
  }
  
  .chart-controls {
    display: flex;
    align-items: center;
    gap: 15px;
  }
  
  .tabs {
    display: flex;
    gap: 5px;
  }
  
  .tabs button {
    padding: 8px 15px;
    background-color: #f1f1f1;
    border: none;
    border-radius: 4px;
    cursor: pointer;
  }
  
  .tabs button.active {
    background-color: #3498db;
    color: white;
  }
  
  .date-selectors {
    display: flex;
    gap: 10px;
  }
  
  select {
    padding: 8px;
    border: 1px solid #ddd;
    border-radius: 4px;
    background-color: white;
  }
  
  .attendance-chart {
    display: flex;
    height: 300px;
    align-items: flex-end;
    margin-bottom: 20px;
  }
  
  .chart-bars {
    display: flex;
    flex: 1;
    height: 100%;
    align-items: flex-end;
    gap: 2px;
    margin-right: 10px;
  }
  
  .chart-bar-container {
    display: flex;
    flex-direction: column;
    flex: 1;
    align-items: center;
    height: 100%;
  }
  
  .chart-bar {
    width: 100%;
    background-color: #3498db;
    border-radius: 4px 4px 0 0;
    transition: height 0.3s ease;
  }
  
  .chart-label {
    margin-top: 5px;
    font-size: 12px;
    color: #7f8c8d;
  }
  
  .chart-axis {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    height: 100%;
    color: #7f8c8d;
    font-size: 12px;
  }
  
  .chart-summary {
    display: flex;
    justify-content: space-around;
    margin-top: 20px;
    padding-top: 20px;
    border-top: 1px solid #eee;
  }
  
  .summary-item {
    text-align: center;
  }
  
  .summary-label {
    font-size: 14px;
    color: #7f8c8d;
    display: block;
    margin-bottom: 5px;
  }
  
  .summary-value {
    font-size: 18px;
    font-weight: bold;
    color: #2c3e50;
  }
</style>