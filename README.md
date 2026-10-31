<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Student Attendance Tracker</title>
<link rel="stylesheet" href="style.css">
</head>
<body>
<header>
<h1>Student Attendance Tracker</h1>
<p>Global College of Arts and Science | B.Sc. (Computer Science)</p>
</header>
<main>
<section class="summary" aria-label="Attendance summary">
<article class="card"><span>Total Students</span><strong id="total">0</
strong></article>
<article class="card"><span>Present Today</span><strong id="present">0</
strong></article>
<article class="card"><span>Absent Today</span><strong id="absent">0</
strong></article>
<article class="card"><span>Overall Attendance</span><strong id="overall">0%<
/strong></article>
</section>
<section class="panel">
<h2 id="formTitle">Add Student</h2>
<form id="studentForm">
<input type="hidden" id="editOriginalId">
<input id="studentId" placeholder="Student ID (e.g., 36725U18016)" required>
<input id="studentName" placeholder="Student Name" required>
<input id="department" placeholder="Department (e.g., B.Sc. CS)" required>
<button type="submit" id="submitBtn">Add Student</button>
<button type="button" id="cancelEditBtn" class="btn-secondary hidden">Cancel</button>
</form>
</section>
<section class="panel">
<h2>Mark Daily Attendance &amp; Records</h2>
<div class="controls-bar">
<div class="control-group"><label for="attendanceDate">Date:</label><input
id="attendanceDate" type="date" required></div>
<div class="control-group"><label for="threshold">Min Threshold (%):</label><
input id="threshold" type="number" min="0" max="100" value="75" class="small-input"></div>
<div class="control-group grow"><input id="search" placeholder="Search by name, ID or
department..."></div>
<div class="control-group"><select id="statusFilter"><option value="All">All
Statuses</option><option value="Present">Present</option><option value="Absent">
Absent</option><option value="Late">Late</option><option value="Leave">Leave</
option><option value="Not marked">Not Marked</option></select></div>
</div>
<p id="message" role="status"></p>
<div class="table-wrap"><table><thead><tr><th>Student ID</th><
th>Student Name</th><th>Department</th><th>Percentage</th><th>Status (
Selected Date)</th><th>Mark Attendance</th><th>Manage</th></tr></
thead><tbody id="studentRows"></tbody></table></div>
</section>
<section id="historyPanel" class="panel hidden">
<div class="history-header"><h2 id="historyTitle">Attendance History</h2><button
type="button" id="closeHistoryBtn" class="btn-secondary">Close</button></div>
<div class="table-wrap"><table><thead><tr><th>Date</th><th>
Status</th><th>Action</th></tr></thead><tbody id="historyRows"></
tbody></table></div>
</section>
</main>
<script src="script.js"></script>
</body>
</html>
* { box-sizing: border-box; }
body { margin: 0; font-family: Arial, sans-serif; background: #f3f6fb; color: #263548; }
header { background: #173656; color: white; padding: 24px 5%; }
header h1 { margin: 0 0 8px; }
header p { margin: 0; color: #dce9f8; }
main { max-width: 1200px; margin: 24px auto; padding: 18px; }
.summary { display: grid; grid-template-columns: repeat(4, 1fr); gap: 14px; margin-bottom: 20px; }
.card, .panel { background: white; border: 1px solid #d8e1ed; border-radius: 12px; padding: 18px; }
.card span { display: block; color: #65758b; font-size: 13px; text-transform: uppercase; }
.card strong { display: block; font-size: 28px; margin-top: 10px; color: #1c4e80; }
.panel { margin-bottom: 18px; }
.panel h2 { margin-top: 0; font-size: 18px; color: #173656; }
form, .controls-bar { display: flex; flex-wrap: wrap; gap: 12px; align-items: center; }
.control-group { display: flex; align-items: center; gap: 6px; }
.control-group.grow { flex: 1; }
.control-group.grow input { width: 100%; }
input, select, button { padding: 10px 12px; border: 1px solid #cbd6e4; border-radius: 7px; font-size: 14px; }
input { min-width: 150px; }
input.small-input { min-width: 70px; width: 75px; }
button { background: #2f75b5; color: white; border: 0; cursor: pointer; transition: background 0.2s; }
button:hover { background: #245d91; }
.btn-present { background: #217346; } .btn-present:hover { background: #195935; }
.btn-absent { background: #b42318; } .btn-absent:hover { background: #8c1b12; }
.btn-late { background: #d97706; } .btn-late:hover { background: #b45309; }
.btn-leave { background: #64748b; } .btn-leave:hover { background: #475569; }
.btn-secondary { background: #e2e8f0; color: #1e293b; } .btn-secondary:hover { background: #cbd5e1; }
.btn-danger { background: #fee2e2; color: #b42318; } .btn-danger:hover { background: #fecaca; }
.btn-sm { padding: 6px 10px; font-size: 12px; margin-right: 4px; }
.table-wrap { overflow-x: auto; margin-top: 14px; }
table { width: 100%; border-collapse: collapse; background: white; }
th, td { padding: 12px; text-align: left; border-bottom: 1px solid #e4eaf2; font-size: 14px; }
th { background: #eaf1f9; color: #173656; }
tr.low-row { background-color: #fffbfa; }
.status-present { color: #217346; font-weight: bold; }
.status-absent, .low { color: #b42318; font-weight: bold; }
.status-late { color: #d97706; font-weight: bold; }
.status-leave { color: #64748b; font-weight: bold; }
#message { min-height: 20px; margin: 10px 0 0; font-weight: bold; color: #217346; }
#message.error { color: #b42318; }
.hidden { display: none; }
.history-header { display: flex; justify-content: space-between; align-items: center; }
@media (max-width: 760px) { .summary { grid-template-columns: repeat(2, 1fr); } form, .controls-bar { flex-
direction: column; align-items: stretch; } input, select, button { width: 100%; } }
@media (max-width: 420px) { .summary { grid-template-columns: 1fr; } }

