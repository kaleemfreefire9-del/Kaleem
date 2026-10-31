
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
 <article class="card"><span>Total Students</span><strong id="total">0</strong></article>
 <article class="card"><span>Present Today</span><strong id="present">0</strong></article>
 <article class="card"><span>Absent Today</span><strong id="absent">0</strong></article>
 <article class="card"><span>Overall Attendance</span><strong id="overall">0%</strong></article>
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
 <div class="control-group"><label for="attendanceDate">Date:</label><input id="attendanceDate" type="date" required></div>
 <div class="control-group"><label for="threshold">Min Threshold (%):</label><input id="threshold" type="number" min="0" max="100" value="75" class="small-input"></div>
 <div class="control-group grow"><input id="search" placeholder="Search by name, ID or department..."></div>
 <div class="control-group"><select id="statusFilter"><option value="All">All Statuses</option><option value="Present">Present</option><option value="Absent">Absent</option><option value="Late">Late</option><option value="Leave">Leave</option><option value="Not marked">Not Marked</option></select></div>
 </div>
 <p id="message" role="status"></p>
 <div class="table-wrap"><table><thead><tr><th>Student ID</th><th>Student Name</th><th>Department</th><th>Percentage</th><th>Status (Selected Date)</th><th>Mark Attendance</th><th>Manage</th></tr></thead><tbody id="studentRows"></tbody></table></div>
 </section>
 <section id="historyPanel" class="panel hidden">
 <div class="history-header"><h2 id="historyTitle">Attendance History</h2><button type="button" id="closeHistoryBtn" class="btn-secondary">Close</button></div>
 <div class="table-wrap"><table><thead><tr><th>Date</th><th>Status</th><th>Action</th></tr></thead><tbody id="historyRows"></tbody></table></div>
 </section>
 </main>
 <script src="script.js"></script>
</body>
</html>
