

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>To-Do List</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div class="container">
        <h1>To-Do List</h1>
        <input type="text" id="taskInput" placeholder="Add a new task">
        <button id="addTaskBtn">Add Task</button>
        <ul id="taskList"></ul>
    </div>
    <script src="script.js"></script>
</body>
</html>
body {
    font-family: Arial, sans-serif;
    margin: 0;
    padding: 0;
}

.container {
    max-width: 600px;
    margin: 50px auto;
    padding: 0 20px;
}

h1 {
    text-align: center;
}

input[type="text"] {
    width: 70%;
    padding: 10px;
    margin-bottom: 10px;
}

button {
    padding: 10px 20px;
    background-color: #007bff;
    color: #fff;
    border: none;
    cursor: pointer;
}

ul {
    list-style-type: none;
    padding: 0;
}

li {
    padding: 10px;
    background-color: #f9f9f9;
    margin-bottom: 5px;
}

.completed {
    text-decoration: line-through;
    opacity: 0.5;
}
document.addEventListener('DOMContentLoaded', function() {
    const taskInput = document.getElementById('taskInput');
    const addTaskBtn = document.getElementById('addTaskBtn');
    const taskList = document.getElementById('taskList');

    // Load tasks from local storage
    let tasks = JSON.parse(localStorage.getItem('tasks')) || [];

    // Display tasks
    function displayTasks() {
        taskList.innerHTML = '';
        tasks.forEach(function(task, index) {
            const li = document.createElement('li');
            li.innerHTML = `
                <span class="${task.completed ? 'completed' : ''}">${task.text}</span>
                <button class="editBtn">Edit</button>
                <button class="deleteBtn">Delete</button>
            `;
            li.querySelector('.editBtn').addEventListener('click', function() {
                editTask(index);
            });
            li.querySelector('.deleteBtn').addEventListener('click', function() {
                deleteTask(index);
            });
            li.querySelector('span').addEventListener('click', function() {
                toggleCompleted(index);
            });
            taskList.appendChild(li);
        });
    }

    // Add task
    addTaskBtn.addEventListener('click', function() {
        const text = taskInput.value.trim();
        if (text !== '') {
            tasks.push({ text: text, completed: false });
            displayTasks();
            saveTasks();
            taskInput.value = '';
        }
    });

    // Edit task
    function editTask(index) {
        const newText = prompt('Edit task:', tasks[index].text);
        if (newText !== null) {
            tasks[index].text = newText.trim();
            displayTasks();
            saveTasks();
        }
    }

    // Delete task
    function deleteTask(index) {
        tasks.splice(index, 1);
        displayTasks();
        saveTasks();
    }

    // Toggle task completed status
    function toggleCompleted(index) {
        tasks[index].completed = !tasks[index].completed;
        displayTasks();
        saveTasks();
    }

    // Save tasks to local storage
    function saveTasks() {
        localStorage.setItem('tasks', JSON.stringify(tasks));
    }

    // Initial display of tasks
    displayTasks();
});
