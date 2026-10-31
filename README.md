<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Student Attendance Tracker</title>
<style>
:root {
    --primary-color: #1e3a8a;
    --bg-color: #f8fafc;
    --card-bg: #ffffff;
    --text-color: #1e293b;
    --border-color: #cbd5e1;
    --success: #16a34a;
    --danger: #dc2626;
    --warning: #d97706;
    --info: #475569;
}

body {
    font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
    background-color: var(--bg-color);
    color: var(--text-color);
    margin: 0;
    padding: 0;
}

header {
    background-color: var(--primary-color);
    color: white;
    padding: 1.5rem 2rem;
}

header h1 {
    margin: 0 0 0.25rem 0;
    font-size: 1.5rem;
}

header p {
    margin: 0;
    font-size: 0.9rem;
    opacity: 0.9;
}

main {
    max-width: 1200px;
    margin: 2rem auto;
    padding: 0 1rem;
    display: flex;
    flex-direction: column;
    gap: 1.5rem;
}

.summary {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
    gap: 1rem;
}

.card {
    background: var(--card-bg);
    padding: 1.25rem;
    border-radius: 8px;
    box-shadow: 0 1px 3px rgba(0,0,0,0.1);
    display: flex;
    flex-direction: column;
    gap: 0.5rem;
}

.card span {
    font-size: 0.75rem;
    font-weight: 700;
    color: #64748b;
    letter-spacing: 0.05em;
}

.card strong {
    font-size: 1.75rem;
    color: var(--primary-color);
}

.panel {
    background: var(--card-bg);
    padding: 1.5rem;
    border-radius: 8px;
    box-shadow: 0 1px 3px rgba(0,0,0,0.1);
}

.panel h2 {
    margin-top: 0;
    font-size: 1.1rem;
    color: var(--primary-color);
    margin-bottom: 1rem;
}

form {
    display: flex;
    gap: 0.75rem;
    flex-wrap: wrap;
}

form input {
    flex: 1;
    min-width: 200px;
    padding: 0.6rem 0.8rem;
    border: 1px solid var(--border-color);
    border-radius: 6px;
    font-size: 0.9rem;
}

button {
    padding: 0.6rem 1.2rem;
    border: none;
    border-radius: 6px;
    background-color: #2563eb;
    color: white;
    font-weight: 500;
    cursor: pointer;
}

button:hover {
    background-color: #1d4ed8;
}

.controls-bar {
    display: flex;
    gap: 1rem;
    align-items: center;
    flex-wrap: wrap;
    margin-bottom: 1.25rem;
}

.control-group {
    display: flex;
    align-items: center;
    gap: 0.5rem;
    font-size: 0.9rem;
}

.control-group.grow {
    flex: 1;
    min-width: 250px;
}

.control-group input, .control-group select {
    padding: 0.5rem;
    border: 1px solid var(--border-color);
    border-radius: 6px;
    font-size: 0.9rem;
}

.control-group.grow input {
    width: 100%;
}

.small-input {
    width: 60px;
}

.table-wrap {
    overflow-x: auto;
}

table {
    width: 100%;
    border-collapse: collapse;
    text-align: left;
    font-size: 0.9rem;
}

th {
    background-color: #eff6ff;
    color: var(--primary-color);
    padding: 0.75rem;
    font-weight: 600;
}

td {
    padding: 0.75rem;
    border-bottom: 1px solid #e2e8f0;
}

.badge-present { color: var(--success); font-weight: 600; }
.badge-absent { color: var(--danger); font-weight: 600; }
.badge-late { color: var(--warning); font-weight: 600; }
.badge-low { color: var(--danger); font-size: 0.85rem; }

.btn-present { background-color: var(--success); }
.btn-absent { background-color: var(--danger); }
.btn-late { background-color: var(--warning); }
.btn-leave { background-color: var(--info); }

.action-btn {
    padding: 0.3rem 0.6rem;
    font-size: 0.8rem;
    border-radius: 4px;
    margin-right: 0.25rem;
}

.btn-history { background-color: #e2e8f0; color: #334155; }
.btn-edit { background-color: #e2e8f0; color: #334155; }
.btn-delete { background-color: #fee2e2; color: #991b1b; }
</style>
</head>
<body>
<header>
<h1>Student Attendance Tracker</h1>
<p>Global College of Arts and Science | B.Sc. (Computer Science)</p>
</header>
<main>
<section class="summary" aria-label="Attendance summary">
<article class="card"><span>TOTAL STUDENTS</span><strong id="total">4</strong></article>
<article class="card"><span>PRESENT TODAY</span><strong id="present">2</strong></article>
<article class="card"><span>ABSENT TODAY</span><strong id="absent">1</strong></article>
<article class="card"><span>OVERALL ATTENDANCE</span><strong id="overall">71%</strong></article>
</section>

<section class="panel">
<h2 id="formTitle">Add Student</h2>
<form id="studentForm">
<input type="hidden" id="editOriginalId">
<input id="studentId" placeholder="Student ID (e.g., 36725U18016)" required>
<input id="studentName" placeholder="Student Name" required>
<input id="department" placeholder="Department (e.g., B.Sc. CS)" required>
<button type="submit" id="submitBtn">Add Student</button>
</form>
</section>

<section class="panel">
<h2>Mark Daily Attendance &amp; Records</h2>
<div class="controls-bar">
<div class="control-group"><label for="attendanceDate">Date:</label><input id="attendanceDate" type="date" value="2026-10-31" required></div>
<div class="control-group"><label for="threshold">Min Threshold (%):</label><input id="threshold" type="number" min="0" max="100" value="75" class="small-input"></div>
<div class="control-group grow"><input id="search" placeholder="Search by name, ID or department..."></div>
<div class="control-group">
<select id="statusFilter">
<option value="All">All Statuses</option>
<option value="Present">Present</option>
<option value="Absent">Absent</option>
<option value="Late">Late</option>
<option value="Leave">Leave</option>
<option value="Not marked">Not Marked</option>
</select>
</div>
</div>
<p id="message" role="status"></p>
<div class="table-wrap">
<table>
<thead>
<tr>
<th>Student ID</th>
<th>Student Name</th>
<th>Department</th>
<th>Percentage</th>
<th>Status (Selected Date)</th>
<th>Mark Attendance</th>
<th>Manage</th>
</tr>
</thead>
<tbody id="studentRows">
<tr>
<td>36725U18016</td>
<td>S. Mohammed Kaleem</td>
<td>B.Sc. CS</td>
<td>100%</td>
<td><span class="badge-present">Present</span></td>
<td>
<button class="action-btn btn-present">Present</button>
<button class="action-btn btn-absent">Absent</button>
<button class="action-btn btn-late">Late</button>
<button class="action-btn btn-leave">Leave</button>
</td>
<td>
<button class="action-btn btn-history">History</button>
<button class="action-btn btn-edit">Edit</button>
<button class="action-btn btn-delete">Delete</button>
</td>
</tr>
<tr>
<td>36725U18017</td>
<td>A. Rahim</td>
<td>B.Sc. CS</td>
<td>33% <span class="badge-low">(Low)</span></td>
<td><span class="badge-absent">Absent</span></td>
<td>
<button class="action-btn btn-present">Present</button>
<button class="action-btn btn-absent">Absent</button>
<button class="action-btn btn-late">Late</button>
<button class="action-btn btn-leave">Leave</button>
</td>
<td>
<button class="action-btn btn-history">History</button>
<button class="action-btn btn-edit">Edit</button>
<button class="action-btn btn-delete">Delete</button>
</td>
</tr>
<tr>
<td>36725U18018</td>
<td>Priya S</td>
<td>B.Sc. Maths</td>
<td>50% <span class="badge-low">(Low)</span></td>
<td><span class="badge-late">Late</span></td>
<td>
<button class="action-btn btn-present">Present</button>
<button class="action-btn btn-absent">Absent</button>
<button class="action-btn btn-late">Late</button>
<button class="action-btn btn-leave">Leave</button>
</td>
<td>
<button class="action-btn btn-history">History</button>
<button class="action-btn btn-edit">Edit</button>
<button class="action-btn btn-delete">Delete</button>
</td>
</tr>
<tr>
<td>36725U18019</td>
<td>V. Kumar</td>
<td>B.Sc. CS</td>
<td>100%</td>
<td><span class="badge-present">Present</span></td>
<td>
<button class="action-btn btn-present">Present</button>
<button class="action-btn btn-absent">Absent</button>
<button class="action-btn btn-late">Late</button>
<button class="action-btn btn-leave">Leave</button>
</td>
<td>
<button class="action-btn btn-history">History</button>
<button class="action-btn btn-edit">Edit</button>
<button class="action-btn btn-delete">Delete</button>
</td>
</tr>
</tbody>
</table>
</div>
</section>
</main>
</body>
</html>
