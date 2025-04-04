# day-planner
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Day Planner</title>
    <style>
        body {
            font-family: sans-serif;
            margin: 0;
            padding: 0;
            display: flex;
            flex-direction: column;
            align-items: center;
            background-color: #f4f4f4;
        }

        #planner {
            width: 80%;
            max-width: 800px;
            margin: 20px;
            background-color: white;
            border-radius: 8px;
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
            padding: 20px;
        }

        #date-display {
            text-align: center;
            margin-bottom: 20px;
        }

        .time-slot {
            display: flex;
            align-items: center;
            margin-bottom: 10px;
        }

        .time {
            width: 80px;
            text-align: right;
            margin-right: 10px;
        }

        .task {
            flex-grow: 1;
            padding: 8px;
            border: 1px solid #ddd;
            border-radius: 4px;
            outline: none;
        }

        .save-btn {
            background-color: #4CAF50;
            color: white;
            border: none;
            padding: 8px 12px;
            border-radius: 4px;
            cursor: pointer;
            margin-left: 10px;
        }

        .save-btn:hover {
            background-color: #45a049;
        }

        #date-picker {
          margin-bottom: 10px;
        }
    </style>
</head>
<body>
    <div id="planner">
        <input type="date" id="date-picker">
        <div id="date-display"></div>
        <div id="time-slots"></div>
    </div>

    <script>
        const dateDisplay = document.getElementById('date-display');
        const timeSlots = document.getElementById('time-slots');
        const datePicker = document.getElementById('date-picker');

        let currentDate = new Date();
        displayDate(currentDate);
        generateTimeSlots();

        datePicker.valueAsDate = currentDate;

        datePicker.addEventListener('change', () => {
            currentDate = new Date(datePicker.value);
            displayDate(currentDate);
            generateTimeSlots();
        });

        function displayDate(date) {
            const options = { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' };
            dateDisplay.textContent = date.toLocaleDateString(undefined, options);
        }

        function generateTimeSlots() {
            timeSlots.innerHTML = ''; // Clear existing slots

            for (let hour = 9; hour <= 17; hour++) {
                const timeSlot = document.createElement('div');
                timeSlot.classList.add('time-slot');

                const time = document.createElement('div');
                time.classList.add('time');
                time.textContent = `${hour}:00`;

                const task = document.createElement('textarea');
                task.classList.add('task');
                task.id = `task-${hour}`;

                const saveBtn = document.createElement('button');
                saveBtn.classList.add('save-btn');
                saveBtn.textContent = 'Save';
                saveBtn.addEventListener('click', () => saveTask(hour));

                timeSlot.appendChild(time);
                timeSlot.appendChild(task);
                timeSlot.appendChild(saveBtn);
                timeSlots.appendChild(timeSlot);

                // Load saved task if any
                loadTask(hour);
            }
        }

        function saveTask(hour) {
            const task = document.getElementById(`task-${hour}`).value;
            const dateKey = currentDate.toISOString().split('T')[0];
            localStorage.setItem(`${dateKey}-${hour}`, task);
        }

        function loadTask(hour) {
            const dateKey = currentDate.toISOString().split('T')[0];
            const savedTask = localStorage.getItem(`${dateKey}-${hour}`);
            if (savedTask) {
                document.getElementById(`task-${hour}`).value = savedTask;
            }
        }
    </script>
</body>
</html>
