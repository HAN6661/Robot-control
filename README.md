# Robot-control
### To create a simple web interface using XAMPP and Visual Studio Code (VS Code) to control a robot and display its last movement, you'll follow these steps:

### 1. Set Up the Environment
XAMPP: Install XAMPP on your system to set up a local Apache server with MySQL and PHP.

### 2. Create the Database
MySQL Setup: Use phpMyAdmin (included with XAMPP) to create a database and table to store the robot's movement commands.

```
CREATE TABLE `movement` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `direction` varchar(50) NOT NULL,
  `timestamp` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`)
);
```

### 3. Create the Control Page
This page will contain buttons to send movement commands (forward, backward, left, right) to the database.

### 4.Create Files
Create the following files in the robot_control folder:

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Last Robot Movement</title>
    <style>
        .data-display {
            font-size: 24px;
            margin: 20px;
        }
    </style>
</head>
<body>
    <h1>Last Recorded Movement</h1>
    <div class="data-display">
        Direction: <span id="direction">Loading...</span><br>
        Timestamp: <span id="timestamp">Loading...</span>
    </div>

    <script>
        // Function to fetch and display the last movement
        function fetchLastMovement() {
            fetch('/last-movement')
                .then(response => response.json())
                .then(data => {
                    document.getElementById('direction').innerText = data.direction;
                    document.getElementById('timestamp').innerText = data.timestamp;
                })
                .catch(error => {
                    document.getElementById('direction').innerText = 'Error loading data';
                    document.getElementById('timestamp').innerText = 'Error loading data';
                    console.error('Error:', error);
                });
        }

        // Fetch the last movement on page load
        window.onload = fetchLastMovement;
    </script>
</body>
</html>

```

### 5.Updated Display Page (display.php)
```
<?php
// Database connection
$servername = "localhost";
$username = "root";
$password = "";
$dbname = "robot_control";

$conn = new mysqli($servername, $username, $password, $dbname);

if ($conn->connect_error) {
    die("Connection failed: " . $conn->connect_error);
}

$sql = "SELECT direction, timestamp FROM movements ORDER BY id DESC LIMIT 1";
$result = $conn->query($sql);

if ($result->num_rows > 0) {
    $row = $result->fetch_assoc();
    $direction = $row['direction'];
    $timestamp = $row['timestamp'];
} else {
    $direction = "No data";
    $timestamp = "No data";
}

$conn->close();
?>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Last Movement - Student Project</title>
    <style>
        body {
            background-color: #fffaf0;
            font-family: Arial, sans-serif;
            text-align: center;
        }
        h1 {
            color: #ff4500;
            font-size: 36px;
            margin-top: 50px;
        }
        .data-display {
            font-size: 24px;
            margin: 20px;
            color: #2f4f4f;
        }
        .data-display span {
            font-weight: bold;
            color: #000080;
        }
    </style>
</head>
<body>
    <h1>Last Recorded Movement</h1>
    <div class="data-display">
        Direction: <span id="direction"><?php echo $direction; ?></span><br>
        Timestamp: <span id="timestamp"><?php echo $timestamp; ?></span>
    </div>
</body>
</html>

```

### 5































