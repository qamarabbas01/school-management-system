<script>
  import { onMount, createEventDispatcher } from "svelte";
  import { fade, slide, scale } from "svelte/transition";

  const dispatch = createEventDispatcher();

  export let title = "Exam Management";
  export let exams = [];
  export let students = [];
  export let canManage = true;

  onMount(() => {
    if (exams.length === 0) {
      exams = [
        {
          id: 1,
          name: "Mathematics Mid-Term",
          subject: "Mathematics",
          totalMarks: 100,
          passingMarks: 40,
          date: "2023-04-10",
          duration: 120, // minutes
          status: "completed",
          startTime: "09:00",
          endTime: "11:00",
        },
        {
          id: 2,
          name: "Science Quiz",
          subject: "Science",
          totalMarks: 50,
          passingMarks: 20,
          date: "2023-04-15",
          duration: 45,
          status: "completed",
          startTime: "10:30",
          endTime: "11:15",
        },
        {
          id: 3,
          name: "English Essay",
          subject: "English",
          totalMarks: 100,
          passingMarks: 40,
          date: "2023-04-20",
          duration: 90,
          status: "ongoing",
          startTime: "13:00",
          endTime: "14:30",
        },
        {
          id: 4,
          name: "History Final",
          subject: "History",
          totalMarks: 100,
          passingMarks: 40,
          date: "2023-04-25",
          duration: 120,
          status: "upcoming",
          startTime: "09:00",
          endTime: "11:00",
        },
        {
          id: 5,
          name: "Computer Science Practical",
          subject: "Computer Science",
          totalMarks: 75,
          passingMarks: 30,
          date: "2023-04-28",
          duration: 180,
          status: "upcoming",
          startTime: "09:00",
          endTime: "12:00",
        },
        {
          id: 6,
          name: "Geography Project",
          subject: "Geography",
          totalMarks: 100,
          passingMarks: 40,
          date: "2023-05-02",
          duration: 60,
          status: "upcoming",
          startTime: "10:00",
          endTime: "11:00",
        },
        {
          id: 7,
          name: "Physical Education Test",
          subject: "Physical Education",
          totalMarks: 50,
          passingMarks: 20,
          date: "2023-05-05",
          duration: 30,
          status: "upcoming",
          startTime: "14:00",
          endTime: "14:30",
        },
        {
          id: 8,
          name: "Art and Craft Exhibition",
          subject: "Art",
          totalMarks: 100,
          passingMarks: 40,
          date: "2023-05-10",
          duration: 120,
          status: "upcoming",
          startTime: "09:00",
          endTime: "11:00",
        },
        {
          id: 9,
          name: "Music Theory Exam",
          subject: "Music",
          totalMarks: 50,
          passingMarks: 20,
          date: "2023-05-15",
          duration: 45,
          status: "upcoming",
          startTime: "10:30",
          endTime: "11:15",
        },
        {
          id: 10,
          name: "Physical Education Final",
          subject: "Physical Education",
          totalMarks: 100,
          passingMarks: 40,
          date: "2023-05-20",
          duration: 120,
          status: "upcoming",
          startTime: "09:00",
          endTime: "11:00",
        },
      ];
    }

    if (students.length === 0) {
      students = [
        {
          id: 1,
          name: "Alice Johnson",
          avatar: "https://i.pravatar.cc/150?img=1",
          rollNumber: "A001",
          results: [
            { examId: 1, marks: 85, grade: "A", remarks: "Excellent work" },
            { examId: 2, marks: 42, grade: "B", remarks: "Good effort" },
          ],
        },
        {
          id: 2,
          name: "Bob Smith",
          avatar: "https://i.pravatar.cc/150?img=2",
          rollNumber: "A002",
          results: [
            { examId: 1, marks: 72, grade: "B", remarks: "Good understanding" },
            {
              examId: 2,
              marks: 38,
              grade: "C",
              remarks: "Needs improvement in theory",
            },
          ],
        },
        {
          id: 3,
          name: "Charlie Brown",
          avatar: "https://i.pravatar.cc/150?img=3",
          rollNumber: "A003",
          results: [
            {
              examId: 1,
              marks: 95,
              grade: "A+",
              remarks: "Outstanding performance",
            },
            {
              examId: 2,
              marks: 48,
              grade: "A",
              remarks: "Excellent grasp of concepts",
            },
          ],
        },
        {
          id: 4,
          name: "Diana Ross",
          avatar: "https://i.pravatar.cc/150?img=4",
          rollNumber: "A004",
          results: [
            {
              examId: 1,
              marks: 35,
              grade: "F",
              remarks: "Needs significant improvement",
            },
            { examId: 2, marks: 22, grade: "C", remarks: "Barely passing" },
          ],
        },
        {
          id: 5,
          name: "Edward Chen",
          avatar: "https://i.pravatar.cc/150?img=5",
          rollNumber: "A005",
          results: [
            { examId: 1, marks: 78, grade: "B+", remarks: "Good work overall" },
            {
              examId: 2,
              marks: 45,
              grade: "A",
              remarks: "Excellent understanding",
            },
          ],
        },
        {
          id: 6,
          name: "Fiona Miller",
          avatar: "https://i.pravatar.cc/150?img=6",
          rollNumber: "A006",
          results: [
            {
              examId: 1,
              marks: 65,
              grade: "C+",
              remarks: "Average performance",
            },
            {
              examId: 2,
              marks: 30,
              grade: "C",
              remarks: "Needs to focus more",
            },
          ],
        },
        {
          id: 7,
          name: "George Wilson",
          avatar: "https://i.pravatar.cc/150?img=7",
          rollNumber: "A007",
          results: [
            { examId: 1, marks: 92, grade: "A+", remarks: "Exceptional work" },
            {
              examId: 2,
              marks: 47,
              grade: "A",
              remarks: "Very good understanding",
            },
          ],
        },
        {
          id: 8,
          name: "Hannah Garcia",
          avatar: "https://i.pravatar.cc/150?img=8",
          rollNumber: "A008",
          results: [
            { examId: 1, marks: 58, grade: "C", remarks: "Needs improvement" },
            {
              examId: 2,
              marks: 28,
              grade: "D",
              remarks: "Struggling with concepts",
            },
          ],
        },
        {
          id: 9,
          name: "Ian Taylor",
          avatar: "https://i.pravatar.cc/150?img=9",
          rollNumber: "A009",
          results: [
            { examId: 1, marks: 88, grade: "A", remarks: "Excellent work" },
            {
              examId: 2,
              marks: 44,
              grade: "B+",
              remarks: "Good understanding",
            },
          ],
        },
        {
          id: 10,
          name: "Julia Martinez",
          avatar: "https://i.pravatar.cc/150?img=10",
          rollNumber: "A010",
          results: [
            { examId: 1, marks: 75, grade: "B", remarks: "Good effort" },
            { examId: 2, marks: 40, grade: "B", remarks: "Solid performance" },
          ],
        },
        {
          id: 11,
          name: "Kevin Lee",
          avatar: "https://i.pravatar.cc/150?img=11",
          rollNumber: "A011",
          results: [
            {
              examId: 1,
              marks: 62,
              grade: "C+",
              remarks: "Average performance",
            },
            {
              examId: 2,
              marks: 32,
              grade: "C",
              remarks: "Needs more practice",
            },
          ],
        },
        {
          id: 12,
          name: "Laura Thompson",
          avatar: "https://i.pravatar.cc/150?img=12",
          rollNumber: "A012",
          results: [
            { examId: 1, marks: 90, grade: "A+", remarks: "Outstanding work" },
            {
              examId: 2,
              marks: 46,
              grade: "A",
              remarks: "Excellent understanding",
            },
          ],
        },
      ];
    }
  });

  let selectedExam = null;
  let searchQuery = "";
  let sortBy = "name";
  let sortOrder = "asc";
  let filterStatus = "all";
  let showAddExamModal = false;
  let showStartExamModal = false;
  let showEndExamModal = false;
  let showResultModal = false;
  let selectedStudent = null;
  let newExam = createEmptyExam();
  let newResult = { marks: 0, grade: "", remarks: "" };
  let activeTab = "exams";

  function createEmptyExam() {
    return {
      name: "",
      subject: "",
      totalMarks: 100,
      passingMarks: 40,
      date: new Date().toISOString().split("T")[0],
      duration: 60,
      status: "upcoming",
      startTime: "09:00",
      endTime: "10:00",
    };
  }

  function formatDate(dateString) {
    return new Date(dateString).toLocaleDateString("en-US", {
      weekday: "short",
      month: "short",
      day: "numeric",
      year: "numeric",
    });
  }

  function getStatusClass(status) {
    if (status === "completed") return "completed";
    if (status === "ongoing") return "ongoing";
    return "upcoming";
  }

  function getGradeClass(grade) {
    if (grade === "A+" || grade === "A") return "excellent";
    if (grade === "B+" || grade === "B") return "good";
    if (grade === "C+" || grade === "C") return "average";
    return "poor";
  }

  function calculateGrade(marks, totalMarks) {
    const percentage = (marks / totalMarks) * 100;

    if (percentage >= 90) return "A+";
    if (percentage >= 80) return "A";
    if (percentage >= 70) return "B+";
    if (percentage >= 60) return "B";
    if (percentage >= 50) return "C+";
    if (percentage >= 40) return "C";
    if (percentage >= 33) return "D";
    return "F";
  }

  function startExam(exam) {
    selectedExam = exam;
    showStartExamModal = true;
  }

  function confirmStartExam() {
    if (selectedExam) {
      exams = exams.map((exam) => {
        if (exam.id === selectedExam.id) {
          return { ...exam, status: "ongoing" };
        }
        return exam;
      });

      dispatch("examStarted", { examId: selectedExam.id });
    }

    showStartExamModal = false;
    selectedExam = null;
  }

  function endExam(exam) {
    selectedExam = exam;
    showEndExamModal = true;
  }

  function confirmEndExam() {
    if (selectedExam) {
      exams = exams.map((exam) => {
        if (exam.id === selectedExam.id) {
          return { ...exam, status: "completed" };
        }
        return exam;
      });

      dispatch("examEnded", { examId: selectedExam.id });
    }

    showEndExamModal = false;
    selectedExam = null;
  }

  function addExam() {
    showAddExamModal = true;
    newExam = createEmptyExam();
  }

  function saveExam() {
    const examId =
      exams.length > 0 ? Math.max(...exams.map((e) => e.id)) + 1 : 1;

    exams = [
      ...exams,
      {
        id: examId,
        ...newExam,
      },
    ];

    dispatch("examAdded", { examId });
    showAddExamModal = false;
  }

  function viewResult(student, exam) {
    selectedStudent = student;
    selectedExam = exam;

    const result = student.results.find((r) => r.examId === exam.id);
    if (result) {
      newResult = { ...result };
    } else {
      newResult = { marks: 0, grade: "", remarks: "" };
    }

    showResultModal = true;
  }

  function saveResult() {
    if (selectedStudent && selectedExam) {
      if (!newResult.grade) {
        newResult.grade = calculateGrade(
          newResult.marks,
          selectedExam.totalMarks
        );
      }

      students = students.map((student) => {
        if (student.id === selectedStudent.id) {
          const existingResultIndex = student.results.findIndex(
            (r) => r.examId === selectedExam.id
          );

          if (existingResultIndex >= 0) {
            const updatedResults = [...student.results];
            updatedResults[existingResultIndex] = {
              examId: selectedExam.id,
              ...newResult,
            };

            return {
              ...student,
              results: updatedResults,
            };
          } else {
            return {
              ...student,
              results: [
                ...student.results,
                { examId: selectedExam.id, ...newResult },
              ],
            };
          }
        }
        return student;
      });

      dispatch("resultSaved", {
        studentId: selectedStudent.id,
        examId: selectedExam.id,
        result: newResult,
      });
    }

    showResultModal = false;
    selectedStudent = null;
    selectedExam = null;
  }

  function getStudentResult(student, examId) {
    return student.results.find((r) => r.examId === examId);
  }

  function calculateClassAverage(examId) {
    const results = students
      .map((student) => getStudentResult(student, examId))
      .filter((result) => result);

    if (results.length === 0) return 0;

    const total = results.reduce((sum, result) => sum + result.marks, 0);
    return Math.round(total / results.length);
  }

  function calculatePassPercentage(examId) {
    const exam = exams.find((e) => e.id === examId);
    if (!exam) return 0;

    const results = students
      .map((student) => getStudentResult(student, examId))
      .filter((result) => result);

    if (results.length === 0) return 0;

    const passedCount = results.filter(
      (result) => result.marks >= exam.passingMarks
    ).length;
    return Math.round((passedCount / results.length) * 100);
  }

  function getHighestScore(examId) {
    const results = students
      .map((student) => getStudentResult(student, examId))
      .filter((result) => result);

    if (results.length === 0) return 0;

    return Math.max(...results.map((result) => result.marks));
  }

  function getLowestScore(examId) {
    const results = students
      .map((student) => getStudentResult(student, examId))
      .filter((result) => result);

    if (results.length === 0) return 0;

    return Math.min(...results.map((result) => result.marks));
  }

  $: filteredExams = exams.filter((exam) => {
    if (filterStatus === "all") return true;
    return exam.status === filterStatus;
  });

  $: sortedExams = [...filteredExams].sort((a, b) => {
    let comparison = 0;

    if (sortBy === "name") {
      comparison = a.name.localeCompare(b.name);
    } else if (sortBy === "date") {
      comparison = new Date(a.date) - new Date(b.date);
    } else if (sortBy === "subject") {
      comparison = a.subject.localeCompare(b.subject);
    }

    return sortOrder === "asc" ? comparison : -comparison;
  });

  $: filteredStudents = students.filter((student) => {
    return (
      searchQuery === "" ||
      student.name.toLowerCase().includes(searchQuery.toLowerCase()) ||
      student.rollNumber.toLowerCase().includes(searchQuery.toLowerCase())
    );
  });

  function getExamStats(examId) {
    const exam = exams.find((e) => e.id === examId);
    if (!exam) return null;

    return {
      average: calculateClassAverage(examId),
      passPercentage: calculatePassPercentage(examId),
      highest: getHighestScore(examId),
      lowest: getLowestScore(examId),
    };
  }

  function drawPerformanceChart(canvas, examId) {
    if (!canvas) return;

    const ctx = canvas.getContext("2d");
    if (!ctx) return;

    const exam = exams.find((e) => e.id === examId);
    if (!exam) return;

    ctx.clearRect(0, 0, canvas.width, canvas.height);

    const results = students
      .map((student) => getStudentResult(student, examId))
      .filter((result) => result);

    if (results.length === 0) return;

    const gradeCounts = {
      "A+": 0,
      A: 0,
      "B+": 0,
      B: 0,
      "C+": 0,
      C: 0,
      D: 0,
      F: 0,
    };

    results.forEach((result) => {
      if (gradeCounts[result.grade] !== undefined) {
        gradeCounts[result.grade]++;
      }
    });

    const gradeColors = {
      "A+": "#27ae60",
      A: "#2ecc71",
      "B+": "#3498db",
      B: "#2980b9",
      "C+": "#f39c12",
      C: "#f1c40f",
      D: "#e67e22",
      F: "#e74c3c",
    };

    const barWidth = 30;
    const spacing = 15;
    const maxHeight = 150;
    const baseline = canvas.height - 30;

    const maxCount = Math.max(...Object.values(gradeCounts));

    let x = 40;
    Object.entries(gradeCounts).forEach(([grade, count]) => {
      const height = count > 0 ? (count / maxCount) * maxHeight : 0;

      ctx.fillStyle = gradeColors[grade] || "#95a5a6";
      ctx.fillRect(x, baseline - height, barWidth, height);

      ctx.fillStyle = "#2c3e50";
      ctx.font = "12px Arial";
      ctx.textAlign = "center";
      ctx.fillText(grade, x + barWidth / 2, baseline + 15);

      if (count > 0) {
        ctx.fillStyle = "#2c3e50";
        ctx.fillText(count.toString(), x + barWidth / 2, baseline - height - 5);
      }

      x += barWidth + spacing;
    });

    ctx.fillStyle = "#2c3e50";
    ctx.font = "bold 14px Arial";
    ctx.textAlign = "center";
    ctx.fillText("Grade Distribution", canvas.width / 2, 20);

    ctx.strokeStyle = "#95a5a6";
    ctx.beginPath();
    ctx.moveTo(30, 30);
    ctx.lineTo(30, baseline);
    ctx.lineTo(canvas.width - 30, baseline);
    ctx.stroke();
  }
</script>

<div class="exam-container">
  <header>
    <h1>{title}</h1>

    <div class="tabs">
      <button
        class="tab-button {activeTab === 'exams' ? 'active' : ''}"
        on:click={() => (activeTab = "exams")}
      >
        Exams
      </button>
      <button
        class="tab-button {activeTab === 'results' ? 'active' : ''}"
        on:click={() => (activeTab = "results")}
      >
        Results
      </button>
    </div>

    {#if activeTab === "exams"}
      <div class="controls" transition:slide={{ duration: 300 }}>
        <div class="filter-sort">
          <select bind:value={filterStatus} class="filter-select">
            <option value="all">All Exams</option>
            <option value="upcoming">Upcoming</option>
            <option value="ongoing">Ongoing</option>
            <option value="completed">Completed</option>
          </select>

          <div class="sort-controls">
            <span>Sort by:</span>
            <select bind:value={sortBy} class="sort-select">
              <option value="name">Name</option>
              <option value="date">Date</option>
              <option value="subject">Subject</option>
            </select>

            <button
              class="sort-order"
              on:click={() =>
                (sortOrder = sortOrder === "asc" ? "desc" : "asc")}
              title={sortOrder === "asc" ? "Ascending" : "Descending"}
            >
              {sortOrder === "asc" ? "↑" : "↓"}
            </button>
          </div>

          {#if canManage}
            <button class="add-exam-btn" on:click={addExam}>
              + New Exam
            </button>
          {/if}
        </div>
      </div>
    {:else}
      <div class="controls" transition:slide={{ duration: 300 }}>
        <div class="search-filter">
          <input
            type="text"
            placeholder="Search students..."
            bind:value={searchQuery}
            class="search-input"
          />
        </div>
      </div>
    {/if}
  </header>

  {#if activeTab === "exams"}
    <div class="exams-list" transition:fade={{ duration: 200 }}>
      {#if sortedExams.length === 0}
        <div class="no-results">
          <p>
            No exams found. {canManage ? 'Click "New Exam" to add one.' : ""}
          </p>
        </div>
      {:else}
        {#each sortedExams as exam (exam.id)}
          <div class="exam-card" transition:slide={{ duration: 200 }}>
            <div class="exam-header">
              <div class="exam-title-section">
                <h2 class="exam-title">{exam.name}</h2>
                <span class="exam-subject">{exam.subject}</span>
              </div>
              <div class="exam-status {getStatusClass(exam.status)}">
                {exam.status.charAt(0).toUpperCase() + exam.status.slice(1)}
              </div>
            </div>

            <div class="exam-details">
              <div class="detail-item">
                <span class="detail-label">Date:</span>
                <span class="detail-value">{formatDate(exam.date)}</span>
              </div>
              <div class="detail-item">
                <span class="detail-label">Time:</span>
                <span class="detail-value"
                  >{exam.startTime} - {exam.endTime}</span
                >
              </div>
              <div class="detail-item">
                <span class="detail-label">Duration:</span>
                <span class="detail-value">{exam.duration} minutes</span>
              </div>
              <div class="detail-item">
                <span class="detail-label">Total Marks:</span>
                <span class="detail-value">{exam.totalMarks}</span>
              </div>
              <div class="detail-item">
                <span class="detail-label">Passing Marks:</span>
                <span class="detail-value">{exam.passingMarks}</span>
              </div>
            </div>

            {#if exam.status === "completed"}
              <div class="exam-stats">
                {#if getExamStats(exam.id)}
                  <div class="stats-grid">
                    <div class="stat-item">
                      <span class="stat-value"
                        >{getExamStats(exam.id)?.average || "-"}</span
                      >
                      <span class="stat-label">Average</span>
                    </div>
                    <div class="stat-item">
                      <span class="stat-value"
                        >{getExamStats(exam.id)?.passPercentage || "-"}%</span
                      >
                      <span class="stat-label">Pass Rate</span>
                    </div>
                    <div class="stat-item">
                      <span class="stat-value"
                        >{getExamStats(exam.id)?.highest}</span
                      >
                      <span class="stat-label">Highest</span>
                    </div>
                    <div class="stat-item">
                      <span class="stat-value"
                        >{getExamStats(exam.id)?.lowest}</span
                      >
                      <span class="stat-label">Lowest</span>
                    </div>
                  </div>

                  <div class="chart-container">
                    <canvas
                      width="400"
                      height="200"
                      use:drawPerformanceChart={exam.id}
                    ></canvas>
                  </div>
                {/if}
              </div>
            {/if}

            {#if canManage}
              <div class="exam-actions">
                {#if exam.status === "upcoming"}
                  <button
                    class="action-btn start-btn"
                    on:click={() => startExam(exam)}
                  >
                    Start Exam
                  </button>
                {:else if exam.status === "ongoing"}
                  <button
                    class="action-btn end-btn"
                    on:click={() => endExam(exam)}
                  >
                    End Exam
                  </button>
                {:else}
                  <button
                    class="action-btn view-results-btn"
                    on:click={() => (activeTab = "results")}
                  >
                    View Results
                  </button>
                {/if}
              </div>
            {/if}
          </div>
        {/each}
      {/if}
    </div>
  {:else}
    <div class="results-section" transition:fade={{ duration: 200 }}>
      <div class="exam-selector">
        <h3>Select Exam</h3>
        <div class="exam-pills">
          {#each exams.filter((e) => e.status === "completed") as exam (exam.id)}
            <button
              class="exam-pill {selectedExam && selectedExam.id === exam.id
                ? 'active'
                : ''}"
              on:click={() => (selectedExam = exam)}
            >
              {exam.name}
            </button>
          {/each}
        </div>
      </div>

      {#if selectedExam}
        <div
          class="results-table-container"
          transition:slide={{ duration: 300 }}
        >
          <h3>Results for {selectedExam.name}</h3>

          {#if filteredStudents.length === 0}
            <div class="no-results">
              <p>No students match your search criteria.</p>
            </div>
          {:else}
            <table class="results-table">
              <thead>
                <tr>
                  <th>Roll No.</th>
                  <th>Student</th>
                  <th>Marks</th>
                  <th>Grade</th>
                  <th>Status</th>
                  {#if canManage}
                    <th>Actions</th>
                  {/if}
                </tr>
              </thead>
              <tbody>
                {#each filteredStudents as student (student.id)}
                  {@const result = getStudentResult(student, selectedExam.id)}
                  <tr
                    class={result
                      ? result.marks >= selectedExam.passingMarks
                        ? "pass"
                        : "fail"
                      : ""}
                  >
                    <td>{student.rollNumber}</td>
                    <td class="student-cell">
                      <img
                        src={student.avatar || "/placeholder.svg"}
                        alt={student.name}
                        class="student-avatar-small"
                      />
                      <span>{student.name}</span>
                    </td>
                    <td
                      >{result
                        ? result.marks
                        : "-"}/{selectedExam.totalMarks}</td
                    >
                    <td>
                      {#if result}
                        <span class="grade-pill {getGradeClass(result.grade)}">
                          {result.grade}
                        </span>
                      {:else}
                        -
                      {/if}
                    </td>
                    <td>
                      {#if result}
                        <span
                          class="status-pill {result.marks >=
                          selectedExam.passingMarks
                            ? 'pass'
                            : 'fail'}"
                        >
                          {result.marks >= selectedExam.passingMarks
                            ? "Pass"
                            : "Fail"}
                        </span>
                      {:else}
                        <span class="status-pill unmarked">Not Marked</span>
                      {/if}
                    </td>
                    {#if canManage}
                      <td>
                        <button
                          class="action-btn small"
                          on:click={() => viewResult(student, selectedExam)}
                        >
                          {result ? "Edit" : "Add"} Result
                        </button>
                      </td>
                    {/if}
                  </tr>
                {/each}
              </tbody>
            </table>
          {/if}

          {#if selectedExam}
            {@const stats = getExamStats(selectedExam.id)}
            <div class="exam-stats">
              {#if stats}
                <div class="stats-summary">
                  <h3>Class Performance Summary</h3>
                  <div class="stats-grid">
                    <div class="stat-item">
                      <span class="stat-value">{stats.average}</span>
                      <span class="stat-label">Class Average</span>
                    </div>
                    <div class="stat-item">
                      <span class="stat-value">{stats.passPercentage}%</span>
                      <span class="stat-label">Pass Percentage</span>
                    </div>
                    <div class="stat-item">
                      <span class="stat-value">{stats.highest}</span>
                      <span class="stat-label">Highest Score</span>
                    </div>
                    <div class="stat-item">
                      <span class="stat-value">{stats.lowest}</span>
                      <span class="stat-label">Lowest Score</span>
                    </div>
                  </div>
                </div>

                <div class="chart-container">
                  <canvas
                    width="400"
                    height="200"
                    use:drawPerformanceChart={selectedExam.id}
                  ></canvas>
                </div>
              {/if}
            </div>
          {/if}
        </div>
      {:else}
        <div class="select-prompt">
          <p>Please select an exam to view results</p>
        </div>
      {/if}
    </div>
  {/if}

  {#if showAddExamModal}
    <div class="modal-backdrop" transition:fade>
      <div class="modal" transition:slide={{ duration: 300 }}>
        <h2>Add New Exam</h2>

        <div class="form-group">
          <label for="exam-name">Exam Name:</label>
          <input
            type="text"
            id="exam-name"
            bind:value={newExam.name}
            placeholder="Enter exam name"
          />
        </div>

        <div class="form-group">
          <label for="exam-subject">Subject:</label>
          <input
            type="text"
            id="exam-subject"
            bind:value={newExam.subject}
            placeholder="Enter subject"
          />
        </div>

        <div class="form-row">
          <div class="form-group">
            <label for="exam-date">Date:</label>
            <input type="date" id="exam-date" bind:value={newExam.date} />
          </div>

          <div class="form-group">
            <label for="exam-duration">Duration (minutes):</label>
            <input
              type="number"
              id="exam-duration"
              bind:value={newExam.duration}
              min="15"
              step="5"
            />
          </div>
        </div>

        <div class="form-row">
          <div class="form-group">
            <label for="exam-start-time">Start Time:</label>
            <input
              type="time"
              id="exam-start-time"
              bind:value={newExam.startTime}
            />
          </div>

          <div class="form-group">
            <label for="exam-end-time">End Time:</label>
            <input
              type="time"
              id="exam-end-time"
              bind:value={newExam.endTime}
            />
          </div>
        </div>

        <div class="form-row">
          <div class="form-group">
            <label for="exam-total-marks">Total Marks:</label>
            <input
              type="number"
              id="exam-total-marks"
              bind:value={newExam.totalMarks}
              min="1"
            />
          </div>

          <div class="form-group">
            <label for="exam-passing-marks">Passing Marks:</label>
            <input
              type="number"
              id="exam-passing-marks"
              bind:value={newExam.passingMarks}
              min="1"
            />
          </div>
        </div>

        <div class="modal-actions">
          <button class="cancel-btn" on:click={() => (showAddExamModal = false)}
            >Cancel</button
          >
          <button
            class="save-btn"
            on:click={saveExam}
            disabled={!newExam.name || !newExam.subject}
          >
            Save Exam
          </button>
        </div>
      </div>
    </div>
  {/if}

  {#if showStartExamModal}
    <div class="modal-backdrop" transition:fade>
      <div class="modal" transition:slide={{ duration: 300 }}>
        <h2>Start Exam</h2>

        {#if selectedExam}
          <p>
            Are you sure you want to start the exam <strong
              >{selectedExam.name}</strong
            >?
          </p>
          <p>
            This will mark the exam as ongoing and allow students to take it.
          </p>

          <div class="exam-details-summary">
            <div class="detail-item">
              <span class="detail-label">Date:</span>
              <span class="detail-value">{formatDate(selectedExam.date)}</span>
            </div>
            <div class="detail-item">
              <span class="detail-label">Time:</span>
              <span class="detail-value"
                >{selectedExam.startTime} - {selectedExam.endTime}</span
              >
            </div>
            <div class="detail-item">
              <span class="detail-label">Duration:</span>
              <span class="detail-value">{selectedExam.duration} minutes</span>
            </div>
          </div>
        {/if}

        <div class="modal-actions">
          <button
            class="cancel-btn"
            on:click={() => (showStartExamModal = false)}>Cancel</button
          >
          <button class="confirm-btn" on:click={confirmStartExam}
            >Start Exam</button
          >
        </div>
      </div>
    </div>
  {/if}

  {#if showEndExamModal}
    <div class="modal-backdrop" transition:fade>
      <div class="modal" transition:slide={{ duration: 300 }}>
        <h2>End Exam</h2>

        {#if selectedExam}
          <p>
            Are you sure you want to end the exam <strong
              >{selectedExam.name}</strong
            >?
          </p>
          <p>
            This will mark the exam as completed and allow you to enter results.
          </p>
        {/if}

        <div class="modal-actions">
          <button class="cancel-btn" on:click={() => (showEndExamModal = false)}
            >Cancel</button
          >
          <button class="confirm-btn" on:click={confirmEndExam}>End Exam</button
          >
        </div>
      </div>
    </div>
  {/if}

  {#if showResultModal}
    <div class="modal-backdrop" transition:fade>
      <div class="modal" transition:slide={{ duration: 300 }}>
        <h2>
          {getStudentResult(selectedStudent, selectedExam.id) ? "Edit" : "Add"} Result
        </h2>

        {#if selectedStudent && selectedExam}
          <div class="result-header">
            <div class="student-info-modal">
              <img
                src={selectedStudent.avatar || "/placeholder.svg"}
                alt={selectedStudent.name}
                class="student-avatar"
              />
              <div>
                <p class="student-name">{selectedStudent.name}</p>
                <p class="student-roll">
                  Roll No: {selectedStudent.rollNumber}
                </p>
              </div>
            </div>

            <div class="exam-info-modal">
              <p class="exam-name">{selectedExam.name}</p>
              <p class="exam-details">
                Total Marks: {selectedExam.totalMarks} | Passing Marks: {selectedExam.passingMarks}
              </p>
            </div>
          </div>

          <div class="form-group">
            <label for="result-marks">Marks Obtained:</label>
            <input
              type="number"
              id="result-marks"
              bind:value={newResult.marks}
              min="0"
              max={selectedExam.totalMarks}
            />
            <div class="marks-percentage">
              {Math.round((newResult.marks / selectedExam.totalMarks) * 100)}%
              <span
                class="status-pill {newResult.marks >= selectedExam.passingMarks
                  ? 'pass'
                  : 'fail'}"
              >
                {newResult.marks >= selectedExam.passingMarks ? "Pass" : "Fail"}
              </span>
            </div>
          </div>

          <div class="form-group">
            <label for="result-grade">Grade:</label>
            <select id="result-grade" bind:value={newResult.grade}>
              <option value="">Auto (based on marks)</option>
              <option value="A+">A+</option>
              <option value="A">A</option>
              <option value="B+">B+</option>
              <option value="B">B</option>
              <option value="C+">C+</option>
              <option value="C">C</option>
              <option value="D">D</option>
              <option value="F">F</option>
            </select>
          </div>

          <div class="form-group">
            <label for="result-remarks">Remarks:</label>
            <textarea
              id="result-remarks"
              bind:value={newResult.remarks}
              placeholder="Enter remarks or feedback"
              rows="3"
            ></textarea>
          </div>
        {/if}

        <div class="modal-actions">
          <button class="cancel-btn" on:click={() => (showResultModal = false)}
            >Cancel</button
          >
          <button class="save-btn" on:click={saveResult}> Save Result </button>
        </div>
      </div>
    </div>
  {/if}
</div>

<style>
  .exam-container {
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
    margin-bottom: 16px;
    font-size: 28px;
  }

  .tabs {
    display: flex;
    margin-bottom: 20px;
    border-bottom: 1px solid #ddd;
  }

  .tab-button {
    padding: 10px 20px;
    background: none;
    border: none;
    border-bottom: 3px solid transparent;
    font-weight: 500;
    color: #7f8c8d;
    cursor: pointer;
    transition: all 0.2s;
  }

  .tab-button:hover {
    color: #2c3e50;
  }

  .tab-button.active {
    color: #3498db;
    border-bottom-color: #3498db;
  }

  .controls {
    margin-bottom: 20px;
  }

  .filter-sort,
  .search-filter {
    display: flex;
    gap: 12px;
    margin-bottom: 16px;
    flex-wrap: wrap;
    align-items: center;
  }

  .filter-select,
  .sort-select {
    padding: 10px 16px;
    border: 1px solid #ddd;
    border-radius: 8px;
    background-color: white;
    font-size: 14px;
  }

  .sort-controls {
    display: flex;
    align-items: center;
    gap: 8px;
  }

  .sort-order {
    width: 36px;
    height: 36px;
    display: flex;
    align-items: center;
    justify-content: center;
    background-color: #f1f1f1;
    border: 1px solid #ddd;
    border-radius: 8px;
    cursor: pointer;
    font-weight: bold;
    transition: all 0.2s;
  }

  .sort-order:hover {
    background-color: #e9e9e9;
  }

  .search-input {
    flex: 1;
    min-width: 200px;
    padding: 10px 16px;
    border: 1px solid #ddd;
    border-radius: 8px;
    font-size: 14px;
  }

  .add-exam-btn {
    margin-left: auto;
    padding: 10px 16px;
    background-color: #3498db;
    color: white;
    border: none;
    border-radius: 8px;
    cursor: pointer;
    font-weight: 500;
    transition: all 0.2s;
  }

  .add-exam-btn:hover {
    background-color: #2980b9;
  }

  .exams-list {
    display: grid;
    gap: 20px;
  }

  .exam-card {
    background-color: white;
    border-radius: 12px;
    overflow: hidden;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.05);
  }

  .exam-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 16px 20px;
    border-bottom: 1px solid #ecf0f1;
  }

  .exam-title {
    margin: 0;
    font-size: 18px;
    color: #2c3e50;
  }

  .exam-subject {
    font-size: 14px;
    color: #7f8c8d;
  }

  .exam-status {
    padding: 6px 12px;
    border-radius: 20px;
    font-size: 14px;
    font-weight: 500;
  }

  .exam-status.upcoming {
    background-color: #e8f4f8;
    color: #3498db;
  }

  .exam-status.ongoing {
    background-color: #fef9e7;
    color: #f1c40f;
  }

  .exam-status.completed {
    background-color: #e6f7e9;
    color: #27ae60;
  }

  .exam-details {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    gap: 16px;
    padding: 20px;
    border-bottom: 1px solid #ecf0f1;
  }

  .detail-item {
    display: flex;
    flex-direction: column;
    gap: 4px;
  }

  .detail-label {
    font-size: 14px;
    color: #7f8c8d;
  }

  .detail-value {
    font-weight: 500;
  }

  .exam-stats {
    padding: 20px;
    border-bottom: 1px solid #ecf0f1;
  }

  .stats-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(100px, 1fr));
    gap: 16px;
    margin-bottom: 20px;
  }

  .stat-item {
    display: flex;
    flex-direction: column;
    align-items: center;
    text-align: center;
    padding: 12px;
    background-color: #f8f9fa;
    border-radius: 8px;
  }

  .stat-value {
    font-size: 24px;
    font-weight: bold;
    color: #2c3e50;
  }

  .stat-label {
    font-size: 14px;
    color: #7f8c8d;
  }

  .chart-container {
    display: flex;
    justify-content: center;
    margin-top: 20px;
  }

  .exam-actions {
    padding: 16px 20px;
    display: flex;
    justify-content: flex-end;
    gap: 12px;
  }

  .action-btn {
    padding: 10px 16px;
    border: none;
    border-radius: 8px;
    cursor: pointer;
    font-weight: 500;
    transition: all 0.2s;
  }

  .action-btn.small {
    padding: 6px 12px;
    font-size: 13px;
  }

  .start-btn {
    background-color: #3498db;
    color: white;
  }

  .start-btn:hover {
    background-color: #2980b9;
  }

  .end-btn {
    background-color: #e74c3c;
    color: white;
  }

  .end-btn:hover {
    background-color: #c0392b;
  }

  .view-results-btn {
    background-color: #27ae60;
    color: white;
  }

  .view-results-btn:hover {
    background-color: #219653;
  }

  .no-results {
    padding: 40px;
    text-align: center;
    color: #7f8c8d;
    background-color: white;
    border-radius: 12px;
  }

  .results-section {
    background-color: white;
    border-radius: 12px;
    padding: 20px;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.05);
  }

  .exam-selector {
    margin-bottom: 20px;
  }

  .exam-selector h3 {
    margin-top: 0;
    margin-bottom: 12px;
    font-size: 16px;
    color: #2c3e50;
  }

  .exam-pills {
    display: flex;
    flex-wrap: wrap;
    gap: 10px;
  }

  .exam-pill {
    padding: 8px 16px;
    background-color: #f1f1f1;
    border: 1px solid #ddd;
    border-radius: 20px;
    cursor: pointer;
    font-size: 14px;
    transition: all 0.2s;
  }

  .exam-pill:hover {
    background-color: #e9e9e9;
  }

  .exam-pill.active {
    background-color: #3498db;
    color: white;
    border-color: #3498db;
  }

  .results-table-container {
    margin-top: 20px;
  }

  .results-table-container h3 {
    margin-top: 0;
    margin-bottom: 16px;
    font-size: 18px;
    color: #2c3e50;
  }

  .results-table {
    width: 100%;
    border-collapse: collapse;
  }

  .results-table th,
  .results-table td {
    padding: 12px 16px;
    text-align: left;
    border-bottom: 1px solid #ecf0f1;
  }

  .results-table th {
    background-color: #f8f9fa;
    font-weight: 600;
    color: #2c3e50;
  }

  .results-table tr:hover {
    background-color: #f8f9fa;
  }

  .results-table tr.pass {
    background-color: rgba(39, 174, 96, 0.05);
  }

  .results-table tr.fail {
    background-color: rgba(231, 76, 60, 0.05);
  }

  .student-cell {
    display: flex;
    align-items: center;
    gap: 10px;
  }

  .student-avatar-small {
    width: 30px;
    height: 30px;
    border-radius: 50%;
    object-fit: cover;
  }

  .grade-pill {
    padding: 4px 8px;
    border-radius: 4px;
    font-size: 13px;
    font-weight: 500;
  }

  .grade-pill.excellent {
    background-color: rgba(39, 174, 96, 0.1);
    color: #27ae60;
  }

  .grade-pill.good {
    background-color: rgba(52, 152, 219, 0.1);
    color: #3498db;
  }

  .grade-pill.average {
    background-color: rgba(241, 196, 15, 0.1);
    color: #f1c40f;
  }

  .grade-pill.poor {
    background-color: rgba(231, 76, 60, 0.1);
    color: #e74c3c;
  }

  .status-pill {
    padding: 4px 8px;
    border-radius: 4px;
    font-size: 13px;
    font-weight: 500;
  }

  .status-pill.pass {
    background-color: rgba(39, 174, 96, 0.1);
    color: #27ae60;
  }

  .status-pill.fail {
    background-color: rgba(231, 76, 60, 0.1);
    color: #e74c3c;
  }

  .status-pill.unmarked {
    background-color: rgba(149, 165, 166, 0.1);
    color: #95a5a6;
  }

  .select-prompt {
    padding: 40px;
    text-align: center;
    color: #7f8c8d;
  }

  .stats-summary {
    margin-top: 30px;
  }

  .stats-summary h3 {
    margin-top: 0;
    margin-bottom: 16px;
    font-size: 18px;
    color: #2c3e50;
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
    max-width: 600px;
    box-shadow: 0 4px 20px rgba(0, 0, 0, 0.15);
    max-height: 90vh;
    overflow-y: auto;
  }

  .modal h2 {
    margin-top: 0;
    color: #2c3e50;
    margin-bottom: 20px;
  }

  .form-group {
    margin-bottom: 16px;
  }

  .form-row {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 16px;
    margin-bottom: 16px;
  }

  .form-group label {
    display: block;
    margin-bottom: 8px;
    font-weight: 500;
    color: #2c3e50;
  }

  input[type="text"],
  input[type="number"],
  input[type="date"],
  input[type="time"],
  select,
  textarea {
    width: 100%;
    padding: 10px 12px;
    border: 1px solid #ddd;
    border-radius: 8px;
    font-family: inherit;
    font-size: 14px;
  }

  textarea {
    resize: vertical;
    min-height: 80px;
  }

  .modal-actions {
    display: flex;
    justify-content: flex-end;
    gap: 12px;
    margin-top: 24px;
  }

  .cancel-btn,
  .confirm-btn,
  .save-btn {
    padding: 10px 16px;
    border: none;
    border-radius: 8px;
    cursor: pointer;
    font-weight: 500;
    transition: all 0.2s;
  }

  .cancel-btn {
    background-color: #f1f1f1;
    color: #333;
  }

  .cancel-btn:hover {
    background-color: #e1e1e1;
  }

  .confirm-btn {
    background-color: #3498db;
    color: white;
  }

  .confirm-btn:hover {
    background-color: #2980b9;
  }

  .save-btn {
    background-color: #27ae60;
    color: white;
  }

  .save-btn:hover {
    background-color: #219653;
  }

  .save-btn:disabled {
    background-color: #95a5a6;
    cursor: not-allowed;
  }

  .exam-details-summary {
    background-color: #f8f9fa;
    padding: 16px;
    border-radius: 8px;
    margin: 16px 0;
  }

  .result-header {
    display: flex;
    justify-content: space-between;
    margin-bottom: 20px;
    padding-bottom: 16px;
    border-bottom: 1px solid #ecf0f1;
  }

  .student-info-modal {
    display: flex;
    align-items: center;
    gap: 12px;
  }

  .student-avatar {
    width: 50px;
    height: 50px;
    border-radius: 50%;
    object-fit: cover;
  }

  .student-name {
    font-weight: 600;
    margin: 0;
    font-size: 16px;
  }

  .student-roll {
    font-size: 14px;
    color: #7f8c8d;
    margin: 4px 0 0 0;
  }

  .exam-info-modal {
    text-align: right;
  }

  .exam-name {
    font-weight: 600;
    margin: 0;
    font-size: 16px;
  }

  .exam-details {
    font-size: 14px;
    color: #7f8c8d;
    margin: 4px 0 0 0;
  }

  .marks-percentage {
    margin-top: 8px;
    display: flex;
    align-items: center;
    gap: 8px;
    font-weight: 500;
  }

  @media (max-width: 768px) {
    .form-row {
      grid-template-columns: 1fr;
      gap: 8px;
    }

    .result-header {
      flex-direction: column;
      gap: 16px;
    }

    .exam-info-modal {
      text-align: left;
    }

    .filter-sort {
      flex-direction: column;
      align-items: stretch;
    }

    .add-exam-btn {
      margin-left: 0;
      width: 100%;
    }

    .sort-controls {
      width: 100%;
      justify-content: space-between;
    }
  }
</style>
