<script>
  import { onMount, createEventDispatcher } from "svelte";
  import { fade, slide } from "svelte/transition";

  // Event dispatcher for external communication
  const dispatch = createEventDispatcher();

  // Props with defaults
  export let title = "Student Attendance Tracker";
  export let students = [];
  export let editable = true;
  export let showFilters = true;
  export let showStats = true;

  // Load sample data if none provided
  onMount(() => {
    if (students.length === 0) {
      students = [
        {
          id: 1,
          name: "Alice Johnson",
          avatar: "https://i.pravatar.cc/150?img=1",
          attendance: [
            { date: "2023-04-03", status: true, note: "" },
            { date: "2023-04-04", status: true, note: "" },
            { date: "2023-04-05", status: true, note: "" },
            { date: "2023-04-06", status: false, note: "Doctor appointment" },
            { date: "2023-04-07", status: true, note: "" },
          ],
          contactInfo: "Parent: 555-123-4567",
        },
        {
          id: 2,
          name: "Bob Smith",
          avatar: "https://i.pravatar.cc/150?img=2",
          attendance: [
            { date: "2023-04-03", status: true, note: "" },
            { date: "2023-04-04", status: false, note: "Sick" },
            { date: "2023-04-05", status: false, note: "Sick" },
            { date: "2023-04-06", status: true, note: "" },
            { date: "2023-04-07", status: true, note: "" },
          ],
          contactInfo: "Parent: 555-234-5678",
        },
        {
          id: 3,
          name: "Charlie Brown",
          avatar: "https://i.pravatar.cc/150?img=3",
          attendance: [
            { date: "2023-04-03", status: true, note: "" },
            { date: "2023-04-04", status: true, note: "" },
            { date: "2023-04-05", status: true, note: "" },
            { date: "2023-04-06", status: true, note: "" },
            { date: "2023-04-07", status: true, note: "" },
          ],
          contactInfo: "Parent: 555-345-6789",
        },
        {
          id: 4,
          name: "Diana Ross",
          avatar: "https://i.pravatar.cc/150?img=4",
          attendance: [
            { date: "2023-04-03", status: false, note: "Family emergency" },
            { date: "2023-04-04", status: false, note: "Family emergency" },
            { date: "2023-04-05", status: true, note: "" },
            { date: "2023-04-06", status: false, note: "Late - traffic" },
            { date: "2023-04-07", status: true, note: "" },
          ],
          contactInfo: "Parent: 555-456-7890",
        },
        {
          id: 5,
          name: "Edward Chen",
          avatar: "https://i.pravatar.cc/150?img=5",
          attendance: [
            { date: "2023-04-03", status: true, note: "" },
            { date: "2023-04-04", status: true, note: "" },
            { date: "2023-04-05", status: false, note: "School event" },
            { date: "2023-04-06", status: true, note: "" },
            { date: "2023-04-07", status: true, note: "" },
          ],
          contactInfo: "Parent: 555-567-8901",
        },
      ];
    }
  });

  // State variables
  let selectedDate = new Date();
  let searchQuery = "";
  let statusFilter = "all";
  let showAbsenceModal = false;
  let selectedStudent = null;
  let absenceNote = "";
  let expandedStudent = null;
  let showCalendarView = false;

  // Format date for display
  function formatDate(date) {
    return new Date(date).toLocaleDateString("en-US", {
      weekday: "short",
      month: "short",
      day: "numeric",
    });
  }

  // Get today's date in YYYY-MM-DD format
  function getTodayString() {
    const today = new Date();
    return today.toISOString().split("T")[0];
  }

  // Check if a student has attendance for today
  function hasAttendanceForToday(student) {
    const todayString = getTodayString();
    return student.attendance.some((record) => record.date === todayString);
  }

  // Mark attendance
  function markAttendance(student, isPresent) {
    const todayString = getTodayString();

    // Check if there's already an entry for today
    const existingIndex = student.attendance.findIndex(
      (record) => record.date === todayString
    );

    if (existingIndex >= 0) {
      // Update existing record
      student.attendance[existingIndex].status = isPresent;
      students = [...students]; // Trigger reactivity
    } else {
      // If marking absent, show modal for note
      if (!isPresent) {
        selectedStudent = student;
        showAbsenceModal = true;
        absenceNote = "";
      } else {
        // Add new attendance record
        student.attendance = [
          ...student.attendance,
          {
            date: todayString,
            status: isPresent,
            note: "",
          },
        ];
        students = [...students]; // Trigger reactivity
      }
    }

    dispatch("attendanceChanged", {
      student,
      date: todayString,
      status: isPresent,
    });
  }

  // Save absence with note
  function saveAbsence() {
    if (selectedStudent) {
      const todayString = getTodayString();
      selectedStudent.attendance = [
        ...selectedStudent.attendance,
        {
          date: todayString,
          status: false,
          note: absenceNote,
        },
      ];
      students = [...students]; // Trigger reactivity

      dispatch("attendanceChanged", {
        student: selectedStudent,
        date: todayString,
        status: false,
        note: absenceNote,
      });
    }

    showAbsenceModal = false;
    selectedStudent = null;
    absenceNote = "";
  }

  // Calculate attendance percentage
  function calculateAttendancePercentage(attendance) {
    if (!attendance.length) return 0;
    const present = attendance.filter((record) => record.status).length;
    return Math.round((present / attendance.length) * 100);
  }

  // Get status class based on attendance percentage
  function getStatusClass(percentage) {
    if (percentage >= 90) return "excellent";
    if (percentage >= 75) return "good";
    if (percentage >= 60) return "average";
    return "poor";
  }

  // Get trend indicator (improving, declining, stable)
  function getTrend(attendance) {
    if (attendance.length < 4) return "stable";

    const firstHalf = attendance.slice(0, Math.floor(attendance.length / 2));
    const secondHalf = attendance.slice(Math.floor(attendance.length / 2));

    const firstHalfPresent =
      firstHalf.filter((record) => record.status).length / firstHalf.length;
    const secondHalfPresent =
      secondHalf.filter((record) => record.status).length / secondHalf.length;

    if (secondHalfPresent - firstHalfPresent > 0.1) return "improving";
    if (firstHalfPresent - secondHalfPresent > 0.1) return "declining";
    return "stable";
  }

  // Filter students based on search and status filter
  $: filteredStudents = students.filter((student) => {
    // Search filter
    const matchesSearch =
      searchQuery === "" ||
      student.name.toLowerCase().includes(searchQuery.toLowerCase());

    // Status filter
    const percentage = calculateAttendancePercentage(student.attendance);
    let matchesStatus = true;

    if (statusFilter === "excellent") {
      matchesStatus = percentage >= 90;
    } else if (statusFilter === "good") {
      matchesStatus = percentage >= 75 && percentage < 90;
    } else if (statusFilter === "average") {
      matchesStatus = percentage >= 60 && percentage < 75;
    } else if (statusFilter === "poor") {
      matchesStatus = percentage < 60;
    }

    return matchesSearch && matchesStatus;
  });

  // Calculate overall statistics
  $: stats = {
    totalStudents: students.length,
    presentToday: students.filter((student) => {
      const todayString = getTodayString();
      const todayRecord = student.attendance.find(
        (record) => record.date === todayString
      );
      return todayRecord && todayRecord.status;
    }).length,
    absentToday: students.filter((student) => {
      const todayString = getTodayString();
      const todayRecord = student.attendance.find(
        (record) => record.date === todayString
      );
      return todayRecord && !todayRecord.status;
    }).length,
    unmarkedToday: students.filter((student) => {
      const todayString = getTodayString();
      return !student.attendance.some((record) => record.date === todayString);
    }).length,
    excellentAttendance: students.filter(
      (student) => calculateAttendancePercentage(student.attendance) >= 90
    ).length,
    poorAttendance: students.filter(
      (student) => calculateAttendancePercentage(student.attendance) < 60
    ).length,
  };

  // Toggle expanded view for a student
  function toggleExpandStudent(student) {
    if (expandedStudent === student.id) {
      expandedStudent = null;
    } else {
      expandedStudent = student.id;
    }
  }

  // Toggle between list and calendar view
  function toggleView() {
    showCalendarView = !showCalendarView;
  }

  // Get the last 5 attendance records for a student
  function getRecentAttendance(student) {
    return [...student.attendance]
      .sort((a, b) => new Date(b.date) - new Date(a.date))
      .slice(0, 5);
  }
</script>

<div class="attendance-container">
  <header>
    <h1>{title}</h1>
    <p class="date">Today: {formatDate(new Date())}</p>

    {#if showFilters}
      <div class="controls" transition:slide={{ duration: 300 }}>
        <div class="search-filter">
          <input
            type="text"
            placeholder="Search students..."
            bind:value={searchQuery}
            class="search-input"
          />

          <select bind:value={statusFilter} class="status-filter">
            <option value="all">All Students</option>
            <option value="excellent">Excellent Attendance</option>
            <option value="good">Good Attendance</option>
            <option value="average">Average Attendance</option>
            <option value="poor">Poor Attendance</option>
          </select>

          <button class="view-toggle" on:click={toggleView}>
            {showCalendarView ? "List View" : "Calendar View"}
          </button>
        </div>
      </div>
    {/if}

    {#if showStats}
      <div class="stats-container" transition:slide={{ duration: 300 }}>
        <div class="stat-card">
          <span class="stat-value">{stats.totalStudents}</span>
          <span class="stat-label">Total Students</span>
        </div>
        <div class="stat-card present">
          <span class="stat-value">{stats.presentToday}</span>
          <span class="stat-label">Present Today</span>
        </div>
        <div class="stat-card absent">
          <span class="stat-value">{stats.absentToday}</span>
          <span class="stat-label">Absent Today</span>
        </div>
        <div class="stat-card unmarked">
          <span class="stat-value">{stats.unmarkedToday}</span>
          <span class="stat-label">Unmarked</span>
        </div>
        <div class="stat-card excellent">
          <span class="stat-value">{stats.excellentAttendance}</span>
          <span class="stat-label">Excellent</span>
        </div>
        <div class="stat-card poor">
          <span class="stat-value">{stats.poorAttendance}</span>
          <span class="stat-label">Poor</span>
        </div>
      </div>
    {/if}
  </header>

  {#if !showCalendarView}
    <!-- List View -->
    <div class="attendance-list">
      <div class="header">
        <span class="student-info">Student</span>
        <span class="attendance-record">Recent Attendance</span>
        <span class="attendance-status">Status</span>
        {#if editable}
          <span class="attendance-actions">Mark Today</span>
        {/if}
      </div>

      {#if filteredStudents.length === 0}
        <div class="no-results" transition:fade>
          <p>No students match your search criteria.</p>
        </div>
      {:else}
        {#each filteredStudents as student (student.id)}
          <div class="student-row" transition:slide={{ duration: 200 }}>
            <button
              class="student-info"
              on:click={() => toggleExpandStudent(student)}
              on:keydown={(e) => e.key === 'Enter' && toggleExpandStudent(student)}
            >
              <img
                src={student.avatar || "/placeholder.svg"}
                alt={student.name}
                class="student-avatar"
              />
              <div>
                <p class="student-name">{student.name}</p>
                <div class="student-meta">
                  <span class="student-id">ID: {student.id}</span>
                  {#if expandedStudent === student.id}
                    <span class="contact-info">{student.contactInfo}</span>
                  {/if}
                </div>
              </div>
              <button class="expand-btn">
                {expandedStudent === student.id ? "▲" : "▼"}
              </button>
            </button>

            <div class="attendance-record">
              {#each getRecentAttendance(student) as record}
                <div
                  class="attendance-day {record.status ? 'present' : 'absent'}"
                  title="{formatDate(record.date)}: {record.status
                    ? 'Present'
                    : 'Absent'} {record.note ? '- ' + record.note : ''}"
                >
                  <span class="day-indicator"
                    >{new Date(record.date).getDate()}</span
                  >
                  <span class="status-indicator"
                    >{record.status ? "✓" : "✗"}</span
                  >
                  {#if record.note}
                    <span class="note-indicator">📝</span>
                  {/if}
                </div>
              {/each}
            </div>

            {#if true}
              {@const percentage = calculateAttendancePercentage(
                student.attendance
              )}
              {@const trend = getTrend(student.attendance)}
              <div class="attendance-status">
                <div class="percentage {getStatusClass(percentage)}">
                  {percentage}%

                  {#if trend === "improving"}
                    <span class="trend improving">↗</span>
                  {:else if trend === "declining"}
                    <span class="trend declining">↘</span>
                  {:else}
                    <span class="trend stable">→</span>
                  {/if}
                </div>
                <div class="status-label {getStatusClass(percentage)}">
                  {percentage >= 90
                    ? "Excellent"
                    : percentage >= 75
                      ? "Good"
                      : percentage >= 60
                        ? "Average"
                        : "Poor"}
                </div>
              </div>
            {/if}

            {#if editable}
              <div class="attendance-actions">
                {#if !hasAttendanceForToday(student)}
                  <button
                    class="present-btn"
                    on:click={() => markAttendance(student, true)}
                    transition:fade
                  >
                    Present
                  </button>
                  <button
                    class="absent-btn"
                    on:click={() => markAttendance(student, false)}
                    transition:fade
                  >
                    Absent
                  </button>
                {:else}
                  <div class="already-marked" transition:fade>
                    Already marked
                  </div>
                {/if}
              </div>
            {/if}

            {#if expandedStudent === student.id}
              <div
                class="expanded-details"
                transition:slide={{ duration: 300 }}
              >
                <h3>Attendance History</h3>
                <div class="history-list">
                  {#each [...student.attendance].sort((a, b) => new Date(b.date) - new Date(a.date)) as record}
                    <div class="history-item">
                      <span class="history-date">{formatDate(record.date)}</span
                      >
                      <span
                        class="history-status {record.status
                          ? 'present'
                          : 'absent'}"
                      >
                        {record.status ? "Present" : "Absent"}
                      </span>
                      {#if record.note}
                        <span class="history-note">{record.note}</span>
                      {/if}
                    </div>
                  {/each}
                </div>
              </div>
            {/if}
          </div>
        {/each}
      {/if}
    </div>
  {:else}
    <!-- Calendar View -->
    <div class="calendar-view" transition:fade>
      <h2>Monthly Attendance View</h2>
      <p class="calendar-info">
        This view shows attendance patterns over time.
      </p>

      <div class="calendar-container">
        <!-- Calendar implementation would go here -->
        <p class="placeholder">Calendar view is under development</p>
      </div>
    </div>
  {/if}

  <div class="legend">
    <div><span class="legend-box excellent"></span> Excellent (90-100%)</div>
    <div><span class="legend-box good"></span> Good (75-89%)</div>
    <div><span class="legend-box average"></span> Average (60-74%)</div>
    <div><span class="legend-box poor"></span> Poor (Below 60%)</div>
  </div>

  <!-- Absence Note Modal -->
  {#if showAbsenceModal}
    <div class="modal-backdrop" transition:fade>
      <div class="modal" transition:slide={{ duration: 300 }}>
        <h2>Record Absence</h2>
        <p>Student: {selectedStudent ? selectedStudent.name : ""}</p>
        <p>Date: {formatDate(new Date())}</p>

        <div class="form-group">
          <label for="absence-note">Reason for absence (optional):</label>
          <textarea
            id="absence-note"
            bind:value={absenceNote}
            placeholder="Enter reason for absence..."
            rows="3"
          ></textarea>
        </div>

        <div class="modal-actions">
          <button class="cancel-btn" on:click={() => (showAbsenceModal = false)}
            >Cancel</button
          >
          <button class="save-btn" on:click={saveAbsence}>Save</button>
        </div>
      </div>
    </div>
  {/if}
</div>

<style>
  .attendance-container {
    font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
    max-width: 1200px;
    margin: 0 auto;
    padding: 20px;
    background-color: #f8f9fa;
    border-radius: 12px;
    box-shadow: 0 4px 20px rgba(0, 0, 0, 0.08);
    color: #333;
  }

  header {
    margin-bottom: 24px;
  }

  h1 {
    color: #2c3e50;
    margin-bottom: 8px;
    font-size: 28px;
  }

  .date {
    font-style: italic;
    color: #7f8c8d;
    margin-bottom: 20px;
  }

  .controls {
    margin-bottom: 20px;
  }

  .search-filter {
    display: flex;
    gap: 12px;
    margin-bottom: 16px;
    flex-wrap: wrap;
  }

  .search-input {
    flex: 1;
    min-width: 200px;
    padding: 10px 16px;
    border: 1px solid #ddd;
    border-radius: 8px;
    font-size: 14px;
  }

  .status-filter {
    padding: 10px 16px;
    border: 1px solid #ddd;
    border-radius: 8px;
    background-color: white;
    font-size: 14px;
  }

  .view-toggle {
    padding: 10px 16px;
    background-color: #f1f1f1;
    border: 1px solid #ddd;
    border-radius: 8px;
    cursor: pointer;
    font-weight: 500;
    transition: all 0.2s;
  }

  .view-toggle:hover {
    background-color: #e9e9e9;
  }

  .stats-container {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
    gap: 16px;
    margin-bottom: 24px;
  }

  .stat-card {
    background-color: white;
    border-radius: 8px;
    padding: 16px;
    text-align: center;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
    display: flex;
    flex-direction: column;
    border-top: 4px solid #6c757d;
  }

  .stat-card.present {
    border-top-color: #27ae60;
  }

  .stat-card.absent {
    border-top-color: #e74c3c;
  }

  .stat-card.unmarked {
    border-top-color: #f39c12;
  }

  .stat-card.excellent {
    border-top-color: #27ae60;
  }

  .stat-card.poor {
    border-top-color: #e74c3c;
  }

  .stat-value {
    font-size: 28px;
    font-weight: bold;
    margin-bottom: 4px;
  }

  .stat-label {
    font-size: 14px;
    color: #6c757d;
  }

  .attendance-list {
    background: white;
    border-radius: 12px;
    overflow: hidden;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.05);
  }

  .header {
    display: grid;
    grid-template-columns: 3fr 2fr 1fr 1fr;
    background-color: #34495e;
    color: white;
    padding: 16px 20px;
    font-weight: 600;
    font-size: 14px;
  }

  .student-row {
    display: grid;
    grid-template-columns: 3fr 2fr 1fr 1fr;
    padding: 16px 20px;
    border-bottom: 1px solid #ecf0f1;
    align-items: center;
    transition: background-color 0.2s;
  }

  .student-row:hover {
    background-color: #f8f9fa;
  }

  .student-row:last-child {
    border-bottom: none;
  }

  .student-info {
    display: flex;
    align-items: center;
    gap: 12px;
    cursor: pointer;
  }

  .student-avatar {
    width: 40px;
    height: 40px;
    border-radius: 50%;
    object-fit: cover;
    border: 2px solid #ecf0f1;
  }

  .student-name {
    font-weight: 600;
    margin: 0;
    font-size: 15px;
  }

  .student-meta {
    display: flex;
    flex-direction: column;
    font-size: 12px;
    color: #7f8c8d;
  }

  .expand-btn {
    margin-left: auto;
    background: none;
    border: none;
    color: #95a5a6;
    cursor: pointer;
    font-size: 12px;
  }

  .attendance-record {
    display: flex;
    gap: 8px;
    flex-wrap: wrap;
  }

  .attendance-day {
    width: 36px;
    height: 36px;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    border-radius: 6px;
    position: relative;
    font-size: 12px;
    transition:
      transform 0.2s,
      box-shadow 0.2s;
  }

  .attendance-day:hover {
    transform: translateY(-2px);
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    z-index: 1;
  }

  .day-indicator {
    font-size: 10px;
    opacity: 0.7;
  }

  .status-indicator {
    font-weight: bold;
  }

  .note-indicator {
    position: absolute;
    top: -5px;
    right: -5px;
    font-size: 10px;
  }

  .present {
    background-color: #e6f7e9;
    color: #27ae60;
    border: 1px solid rgba(39, 174, 96, 0.3);
  }

  .absent {
    background-color: #fdeaea;
    color: #e74c3c;
    border: 1px solid rgba(231, 76, 60, 0.3);
  }

  .attendance-status {
    text-align: center;
  }

  .percentage {
    font-weight: bold;
    font-size: 18px;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 4px;
  }

  .trend {
    font-size: 14px;
  }

  .improving {
    color: #27ae60;
  }

  .declining {
    color: #e74c3c;
  }

  .stable {
    color: #7f8c8d;
  }

  .status-label {
    font-size: 12px;
    padding: 2px 8px;
    border-radius: 12px;
    display: inline-block;
    margin-top: 4px;
  }

  .attendance-actions {
    display: flex;
    gap: 8px;
    justify-content: center;
  }

  .present-btn,
  .absent-btn,
  .save-btn,
  .cancel-btn {
    padding: 8px 12px;
    border: none;
    border-radius: 6px;
    cursor: pointer;
    font-weight: 500;
    transition: all 0.2s;
    font-size: 13px;
  }

  .present-btn {
    background-color: #27ae60;
    color: white;
  }

  .present-btn:hover {
    background-color: #219653;
  }

  .absent-btn {
    background-color: #e74c3c;
    color: white;
  }

  .absent-btn:hover {
    background-color: #c0392b;
  }

  .already-marked {
    font-size: 13px;
    color: #7f8c8d;
    font-style: italic;
  }

  .excellent {
    background-color: rgba(39, 174, 96, 0.1);
    color: #27ae60;
  }

  .good {
    background-color: rgba(52, 152, 219, 0.1);
    color: #3498db;
  }

  .average {
    background-color: rgba(241, 196, 15, 0.1);
    color: #f1c40f;
  }

  .poor {
    background-color: rgba(231, 76, 60, 0.1);
    color: #e74c3c;
  }

  .expanded-details {
    grid-column: 1 / -1;
    background-color: #f8f9fa;
    padding: 16px;
    margin-top: 12px;
    border-radius: 8px;
  }

  .expanded-details h3 {
    margin-top: 0;
    font-size: 16px;
    margin-bottom: 12px;
    color: #2c3e50;
  }

  .history-list {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
    gap: 12px;
  }

  .history-item {
    background-color: white;
    padding: 10px;
    border-radius: 6px;
    box-shadow: 0 1px 3px rgba(0, 0, 0, 0.05);
    display: flex;
    flex-direction: column;
    gap: 4px;
  }

  .history-date {
    font-weight: 500;
    font-size: 13px;
  }

  .history-status {
    font-size: 12px;
    padding: 2px 6px;
    border-radius: 4px;
    display: inline-block;
    width: fit-content;
  }

  .history-note {
    font-size: 12px;
    color: #7f8c8d;
    font-style: italic;
  }

  .legend {
    margin-top: 24px;
    display: flex;
    gap: 16px;
    justify-content: center;
    flex-wrap: wrap;
    font-size: 13px;
  }

  .legend div {
    display: flex;
    align-items: center;
    gap: 6px;
  }

  .legend-box {
    display: inline-block;
    width: 16px;
    height: 16px;
    border-radius: 4px;
  }

  .calendar-view {
    background: white;
    border-radius: 12px;
    padding: 20px;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.05);
    margin-bottom: 24px;
  }

  .calendar-view h2 {
    margin-top: 0;
    color: #2c3e50;
  }

  .calendar-info {
    color: #7f8c8d;
    margin-bottom: 16px;
  }

  .calendar-container {
    min-height: 300px;
    display: flex;
    align-items: center;
    justify-content: center;
    border: 1px dashed #ddd;
    border-radius: 8px;
  }

  .placeholder {
    color: #95a5a6;
    font-style: italic;
  }

  .no-results {
    padding: 40px;
    text-align: center;
    color: #7f8c8d;
  }

  .modal-backdrop {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: rgba(0, 0, 0, 0.5);
    display: flex;
    align-items: center;
    justify-content: center;
    z-index: 1000;
  }

  .modal {
    background-color: white;
    border-radius: 12px;
    padding: 24px;
    width: 90%;
    max-width: 500px;
    box-shadow: 0 4px 20px rgba(0, 0, 0, 0.15);
  }

  .modal h2 {
    margin-top: 0;
    color: #2c3e50;
  }

  .form-group {
    margin: 16px 0;
  }

  .form-group label {
    display: block;
    margin-bottom: 8px;
    font-weight: 500;
  }

  textarea {
    width: 100%;
    padding: 12px;
    border: 1px solid #ddd;
    border-radius: 8px;
    font-family: inherit;
    resize: vertical;
  }

  .modal-actions {
    display: flex;
    justify-content: flex-end;
    gap: 12px;
    margin-top: 20px;
  }

  .cancel-btn {
    background-color: #f1f1f1;
    color: #333;
  }

  .cancel-btn:hover {
    background-color: #e1e1e1;
  }

  .save-btn {
    background-color: #3498db;
    color: white;
  }

  .save-btn:hover {
    background-color: #2980b9;
  }

  @media (max-width: 768px) {
    .header {
      display: none;
    }

    .student-row {
      display: flex;
      flex-direction: column;
      gap: 16px;
      padding: 16px;
    }

    .student-info {
      width: 100%;
    }

    .attendance-record {
      width: 100%;
      justify-content: center;
    }

    .attendance-status {
      display: flex;
      justify-content: center;
      gap: 12px;
      width: 100%;
    }

    .attendance-actions {
      width: 100%;
    }

    .stats-container {
      grid-template-columns: repeat(2, 1fr);
    }

    .search-filter {
      flex-direction: column;
    }

    .search-input,
    .status-filter,
    .view-toggle {
      width: 100%;
    }
  }
</style>
