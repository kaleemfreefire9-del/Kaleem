<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Student Attendance Tracker</title>
  <style>
    * { box-sizing: border-box; }
    body {
      margin: 0;
      font-family: Arial, sans-serif;
      background: #f1f5f9;
      color: #1e293b;
    }
    header {
      background: #173b63;
      color: white;
      padding: 18px 5%;
    }
    main {
      max-width: 1100px;
      margin: 24px auto;
      padding: 0 16px;
    }
    .cards {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      gap: 12px;
      margin: 16px 0;
    }
    .card, .panel {
      background: white;
      border-radius: 12px;
      padding: 16px;
      box-shadow: 0 2px 8px #dbe3ed;
    }
    .card strong {
      display: block;
      font-size: 24px;
      margin-top: 8px;
    }
    .layout {
      display: grid;
      grid-template-columns: 280px 1fr;
      gap: 16px;
    }
    input, select, button {
      width: 100%;
      padding: 10px;
      margin: 5px 0 10px;
      border: 1px solid #cbd5e1;
      border-radius: 6px;
    }
    button {
      background: #2563eb;
      color: white;
      border: 0;
      cursor: pointer;
    }
    td button {
      width: auto;
      padding: 6px 10px;
      margin: 0 4px 0 0;
    }
    table {
      width: 100%;
      border-collapse: collapse;
      font-size: 14px;
    }
    th, td {
      padding: 10px;
      text-align: left;
      border-bottom: 1px solid #e2e8f0;
    }
    th { background: #edf2f7; }
    .present {
      color: #166534;
      background: #dcfce7;
      padding: 4px 8px;
      border-radius: 10px;
    }
    .absent {
      color: #991b1b;
      background: #fee2e2;
      padding: 4px 8px;
      border-radius: 10px;
    }
    @media (max-width: 700px) {
      .cards { grid-template-columns: repeat(2, 1fr); }
      .layout { grid-template-columns: 1fr; }
    }
  </style>
</head>
<body>
  <header>
    <h1>Student Attendance Tracker</h1>
    <p>Manage students and record daily attendance</p>
  </header>
  <main>
    <h2>Attendance Dashboard</h2>
    <div class="cards">
      <div class="card">Total Students<strong id="total">0</strong></div>
      <div class="card">Present Today<strong id="present">0</strong></div>
      <div class="card">Absent Today<strong id="absent">0</strong></div>
      <div class="card">Overall Attendance<strong id="overall">0%</strong></div>
    </div>
    <div class="layout">
      <section class="panel">
        <h3>Add Student</h3>
        <form id="studentForm">
          <label for="studentName">Student name</label>
          <input id="studentName" required placeholder="Enter student name">
          <label for="studentId">Student ID</label>
          <input id="studentId" required placeholder="Enter unique ID">
          <button type="submit">Add Student</button>
        </form>
        <label for="date">Attendance date</label>
        <input id="date" type="date" required>
        <label for="search">Search by name or ID</label>
        <input id="search" placeholder="Type to search">
      </section>
      <section class="panel">
        <h3>Attendance Records</h3>
        <p id="message" aria-live="polite"></p>
        <div style="overflow-x:auto">
          <table>
            <thead>
              <tr>
                <th>Student</th>
                <th>ID</th>
                <th>Status</th>
                <th>Attendance</th>
                <th>Action</th>
              </tr>
            </thead>
            <tbody id="rows"></tbody>
          </table>
        </div>
      </section>
    </div>
  </main>
  <script>
    const load = key => JSON.parse(localStorage.getItem(key) || "[]");
    let students = load("students");
    let attendance = load("attendance");
    const today = new Date().toISOString().slice(0, 10);
    document.getElementById("date").value = today;

    function persist() {
      localStorage.setItem("students", JSON.stringify(students));
      localStorage.setItem("attendance", JSON.stringify(attendance));
    }

    function percent(id) {
      const entries = attendance.filter(a => a.studentId === id);
      if (!entries.length) return 0;
      const presentCount = entries.filter(a => a.status === "Present").length;
      return Math.round((presentCount / entries.length) * 100);
    }

    function mark(id, status) {
      const date = document.getElementById("date").value;
      if (!date) {
        alert("Please choose a date.");
        return;
      }
      if (attendance.some(a => a.studentId === id && a.date === date)) {
        alert("Attendance already exists for this student on this date.");
        return;
      }
      attendance.push({ studentId: id, date, status });
      persist();
      render();
    }

    function render() {
      const date = document.getElementById("date").value;
      const query = document.getElementById("search").value.toLowerCase();
      const todays = attendance.filter(a => a.date === date);

      document.getElementById("total").textContent = students.length;
      document.getElementById("present").textContent = todays.filter(a => a.status === "Present").length;
      document.getElementById("absent").textContent = todays.filter(a => a.status === "Absent").length;

      const totalEntries = attendance.length;
      const presentEntries = attendance.filter(a => a.status === "Present").length;
      document.getElementById("overall").textContent = (totalEntries ? Math.round((presentEntries / totalEntries) * 100) : 0) + "%";

      const visible = students.filter(s => (s.name + " " + s.id).toLowerCase().includes(query));
      document.getElementById("rows").innerHTML = visible.map(s => {
        const status = todays.find(a => a.studentId === s.id)?.status || "Not marked";
        return `<tr>
          <td>${escapeHTML(s.name)}</td>
          <td>${escapeHTML(s.id)}</td>
          <td>${status === "Present" ? '<span class="present">Present</span>' : status === "Absent" ? '<span class="absent">Absent</span>' : status}</td>
          <td>${percent(s.id)}%</td>
          <td>
            <button type="button" onclick="mark('${safeId(s.id)}', 'Present')">Present</button>
            <button type="button" onclick="mark('${safeId(s.id)}', 'Absent')">Absent</button>
          </td>
        </tr>`;
      }).join("");
    }

    function escapeHTML(value) {
      return String(value).replace(/[&<>"']/g, ch => ({
        '&': '&amp;',
        '<': '&lt;',
        '>': '&gt;',
        '"': '&quot;',
        "'": '&#39;'
      }[ch]));
    }

    function safeId(value) {
      return String(value).replace(/\\/g, "\\\\").replace(/'/g, "\\'");
    }

    document.getElementById("studentForm").addEventListener("submit", event => {
      event.preventDefault();
      const name = document.getElementById("studentName").value.trim();
      const id = document.getElementById("studentId").value.trim();
      if (!name || !id) {
        alert("Enter both name and ID.");
        return;
      }
      if (students.some(s => s.id.toLowerCase() === id.toLowerCase())) {
        alert("That student ID already exists.");
        return;
      }
      students.push({ name, id });
      persist();
      event.target.reset();
      render();
    });

    document.getElementById("search").addEventListener("input", render);
    document.getElementById("date").addEventListener("change", render);
    render();
  </script>
</body>
</html>
