# think41saran
think41 frontend development code
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Recurring Event Instance Generator</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      padding: 20px;
      background: #f7f7f7;
    }
    label {
      display: block;
      margin-top: 10px;
    }
    input, select, button {
      margin-top: 5px;
      padding: 6px;
      font-size: 14px;
    }
    button {
      margin-top: 15px;
      background: #007bff;
      color: white;
      border: none;
      cursor: pointer;
    }
    button:hover {
      background: #0056b3;
    }
    .gray {
      color: #aaa;
    }
  </style>
</head>
<body>

  <h2>Recurring Event Instance Generator</h2>

  <label>Event Start Date:</label>
  <input type="date" id="startDate">

  <label>Day of Week:</label>
  <select id="dayOfWeek">
    <option>Sunday</option>
    <option>Monday</option>
    <option>Tuesday</option>
    <option>Wednesday</option>
    <option>Thursday</option>
    <option>Friday</option>
    <option>Saturday</option>
  </select>

  <label>Number of Occurrences:</label>
  <input type="number" id="occurrences" min="1" value="5">

  <h4>View Window</h4>
  <label>Start:</label>
  <input type="date" id="viewStart">
  <label>End:</label>
  <input type="date" id="viewEnd">

  <button onclick="generateInstances()">Generate Instances</button>

  <h3>Generated Events:</h3>
  <ul id="resultList"></ul>

  <script>
    const daysMap = {
      "Sunday": 0,
      "Monday": 1,
      "Tuesday": 2,
      "Wednesday": 3,
      "Thursday": 4,
      "Friday": 5,
      "Saturday": 6
    };

    function generateInstances() {
      const startDateValue = document.getElementById("startDate").value;
      const dayOfWeek = document.getElementById("dayOfWeek").value;
      const occurrences = parseInt(document.getElementById("occurrences").value);
      const viewStartValue = document.getElementById("viewStart").value;
      const viewEndValue = document.getElementById("viewEnd").value;
      const resultList = document.getElementById("resultList");

      resultList.innerHTML = ""; // Clear previous results

      if (!startDateValue) {
        alert("Please select a start date!");
        return;
      }

      let events = [];
      let start = new Date(startDateValue);
      let targetDay = daysMap[dayOfWeek];

      // Move start to the next matching day
      while (start.getDay() !== targetDay) {
        start.setDate(start.getDate() + 1);
      }

      // Generate weekly occurrences
      for (let i = 0; i < occurrences; i++) {
        let eventDate = new Date(start);
        eventDate.setDate(start.getDate() + i * 7);
        events.push(eventDate);
      }

      let viewStart = viewStartValue ? new Date(viewStartValue) : null;
      let viewEnd = viewEndValue ? new Date(viewEndValue) : null;

      events.forEach(date => {
        const li = document.createElement("li");
        li.textContent = date.toDateString();

        // Gray out if outside view window
        if (viewStart && viewEnd) {
          if (date < viewStart || date > viewEnd) {
            li.classList.add("gray");
          }
        }
        resultList.appendChild(li);
      });
    }
  </script>

</body>
</html>
