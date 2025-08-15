---
title: "PHP Fundamentals: Server-Side Web Development"
date: 2024-01-18T14:00:00+08:00
tags: ["php", "web development", "basics", "server-side"]
---

PHP (PHP: Hypertext Preprocessor) is a widely-used server-side scripting language that's especially suited for web development. It's the foundation of many popular content management systems and web applications.

## What Makes PHP Special?

PHP was designed specifically for web development:

- **Server-Side Processing**: Executes on the server before sending HTML to the browser
- **Database Integration**: Excellent support for various databases
- **Web-Focused**: Built-in functions for web development tasks
- **Easy Deployment**: Simple to deploy on most web servers
- **Flexible**: Supports both procedural and object-oriented programming

## Setting Up PHP Development Environment

### Local Development Setup

#### Using XAMPP (Cross-platform)

1. Download XAMPP from [apachefriends.org](https://www.apachefriends.org/)
2. Install and start Apache and MySQL services
3. Create PHP files in the `htdocs` directory
4. Access via `http://localhost/your-file.php`

#### Using PHP Built-in Server

```bash
# Install PHP (varies by OS)
# Ubuntu/Debian: sudo apt install php
# macOS: brew install php
# Windows: Download from php.net

# Start built-in server
php -S localhost:8000

# Or specify document root
php -S localhost:8000 -t /path/to/your/project
```

### Your First PHP Script

Create `index.php`:

```php
<!DOCTYPE html>
<html>
<head>
    <title>My First PHP Page</title>
</head>
<body>
    <h1><?php echo "Hello, PHP World!"; ?></h1>
    
    <?php
    $currentTime = date('Y-m-d H:i:s');
    echo "<p>Current time: " . $currentTime . "</p>";
    ?>
</body>
</html>
```

### Understanding PHP Syntax

```php
<?php
// PHP opening tag - required

// Variables start with $
$name = "John Doe";
$age = 30;
$isActive = true;

// Echo outputs content
echo "Name: " . $name . "<br>";
echo "Age: $age<br>";  // Variables in double quotes are parsed

// Comments
// Single line comment
/* Multi-line
   comment */

// PHP closing tag (optional at end of file)
?>
```

## PHP Data Types and Variables

### Basic Data Types

```php
<?php
// String
$firstName = "John";
$lastName = 'Doe';
$fullName = "$firstName $lastName";  // Variable interpolation

// Integer
$age = 25;
$negativeNumber = -100;

// Float
$price = 19.99;
$pi = 3.14159;

// Boolean
$isLoggedIn = true;
$isAdmin = false;

// Array
$colors = array("red", "green", "blue");
$modernArray = ["apple", "banana", "orange"];

// Associative array (like hash map)
$person = array(
    "name" => "John Doe",
    "age" => 30,
    "city" => "New York"
);

// Modern associative array syntax
$user = [
    "id" => 1,
    "username" => "johndoe",
    "email" => "john@example.com"
];

// NULL
$emptyValue = null;

// Check variable type
echo gettype($age);        // integer
echo gettype($isLoggedIn); // boolean
?>
```

### Variable Scope

```php
<?php
$globalVar = "I'm global";

function testScope() {
    global $globalVar;  // Access global variable
    
    $localVar = "I'm local";
    static $staticVar = 0;  // Retains value between calls
    $staticVar++;
    
    echo $globalVar . "<br>";
    echo $localVar . "<br>";
    echo "Static var: $staticVar<br>";
}

testScope();  // Static var: 1
testScope();  // Static var: 2
?>
```

## Control Structures

### Conditional Statements

```php
<?php
$score = 85;

// If-else
if ($score >= 90) {
    $grade = "A";
} elseif ($score >= 80) {
    $grade = "B";
} elseif ($score >= 70) {
    $grade = "C";
} else {
    $grade = "F";
}

echo "Grade: $grade<br>";

// Switch statement
$day = "Monday";
switch ($day) {
    case "Monday":
    case "Tuesday":
    case "Wednesday":
    case "Thursday":
    case "Friday":
        echo "It's a weekday";
        break;
    case "Saturday":
    case "Sunday":
        echo "It's weekend!";
        break;
    default:
        echo "Invalid day";
}

// Ternary operator
$status = ($age >= 18) ? "Adult" : "Minor";
echo "Status: $status<br>";

// Null coalescing operator (PHP 7+)
$username = $_GET['user'] ?? 'guest';
?>
```

### Loops

```php
<?php
// For loop
echo "<h3>For Loop:</h3>";
for ($i = 1; $i <= 5; $i++) {
    echo "Count: $i<br>";
}

// While loop
echo "<h3>While Loop:</h3>";
$count = 1;
while ($count <= 3) {
    echo "While count: $count<br>";
    $count++;
}

// Do-while loop
echo "<h3>Do-While Loop:</h3>";
$num = 1;
do {
    echo "Number: $num<br>";
    $num++;
} while ($num <= 3);

// Foreach loop for arrays
echo "<h3>Foreach Loop:</h3>";
$fruits = ["apple", "banana", "orange"];
foreach ($fruits as $fruit) {
    echo "Fruit: $fruit<br>";
}

// Foreach with key-value pairs
$person = [
    "name" => "John",
    "age" => 30,
    "city" => "New York"
];

foreach ($person as $key => $value) {
    echo "$key: $value<br>";
}
?>
```

## Functions

### Basic Functions

```php
<?php
// Simple function
function greet($name) {
    return "Hello, $name!";
}

echo greet("World");

// Function with default parameters
function createUser($name, $role = "user", $active = true) {
    return [
        "name" => $name,
        "role" => $role,
        "active" => $active
    ];
}

$user1 = createUser("John");
$user2 = createUser("Admin", "administrator", false);

// Function with variable number of arguments
function sum(...$numbers) {
    $total = 0;
    foreach ($numbers as $number) {
        $total += $number;
    }
    return $total;
}

echo sum(1, 2, 3, 4, 5);  // 15

// Anonymous functions (closures)
$multiply = function($a, $b) {
    return $a * $b;
};

echo $multiply(5, 3);  // 15

// Arrow functions (PHP 7.4+)
$square = fn($x) => $x * $x;
echo $square(4);  // 16
?>
```

### Built-in Functions

```php
<?php
// String functions
$text = "  Hello, World!  ";
echo strlen($text);           // Length
echo trim($text);            // Remove whitespace
echo strtoupper($text);      // Uppercase
echo strtolower($text);      // Lowercase
echo substr($text, 0, 5);    // Substring

// Array functions
$numbers = [3, 1, 4, 1, 5, 9];
echo count($numbers);        // Array length
sort($numbers);             // Sort array
echo max($numbers);         // Maximum value
echo min($numbers);         // Minimum value
echo array_sum($numbers);   // Sum of elements

// Date functions
echo date('Y-m-d H:i:s');           // Current date/time
echo date('Y-m-d', strtotime('+1 week')); // Next week

// Math functions
echo round(3.14159, 2);     // 3.14
echo rand(1, 100);          // Random number
echo sqrt(16);              // Square root
?>
```

## Working with Forms

### HTML Form

```html
<!DOCTYPE html>
<html>
<head>
    <title>User Registration</title>
</head>
<body>
    <form action="process.php" method="POST">
        <label>Name:</label>
        <input type="text" name="name" required><br><br>
        
        <label>Email:</label>
        <input type="email" name="email" required><br><br>
        
        <label>Age:</label>
        <input type="number" name="age" min="1" max="120"><br><br>
        
        <label>Gender:</label>
        <select name="gender">
            <option value="male">Male</option>
            <option value="female">Female</option>
            <option value="other">Other</option>
        </select><br><br>
        
        <label>Interests:</label><br>
        <input type="checkbox" name="interests[]" value="programming"> Programming<br>
        <input type="checkbox" name="interests[]" value="design"> Design<br>
        <input type="checkbox" name="interests[]" value="marketing"> Marketing<br><br>
        
        <input type="submit" value="Register">
    </form>
</body>
</html>
```

### Processing Form Data

```php
<?php
// process.php
if ($_SERVER['REQUEST_METHOD'] == 'POST') {
    // Sanitize and validate input
    $name = trim($_POST['name']);
    $email = filter_var($_POST['email'], FILTER_SANITIZE_EMAIL);
    $age = (int)$_POST['age'];
    $gender = $_POST['gender'];
    $interests = $_POST['interests'] ?? [];
    
    // Validation
    $errors = [];
    
    if (empty($name)) {
        $errors[] = "Name is required";
    }
    
    if (!filter_var($email, FILTER_VALIDATE_EMAIL)) {
        $errors[] = "Invalid email format";
    }
    
    if ($age < 1 || $age > 120) {
        $errors[] = "Age must be between 1 and 120";
    }
    
    if (empty($errors)) {
        // Process successful registration
        echo "<h2>Registration Successful!</h2>";
        echo "<p>Name: " . htmlspecialchars($name) . "</p>";
        echo "<p>Email: " . htmlspecialchars($email) . "</p>";
        echo "<p>Age: $age</p>";
        echo "<p>Gender: " . htmlspecialchars($gender) . "</p>";
        
        if (!empty($interests)) {
            echo "<p>Interests: " . implode(", ", array_map('htmlspecialchars', $interests)) . "</p>";
        }
        
        // Here you would typically save to database
        // saveUserToDatabase($name, $email, $age, $gender, $interests);
        
    } else {
        // Display errors
        echo "<h2>Registration Failed</h2>";
        echo "<ul>";
        foreach ($errors as $error) {
            echo "<li>" . htmlspecialchars($error) . "</li>";
        }
        echo "</ul>";
        echo '<a href="register.html">Go back</a>';
    }
}
?>
```

## Object-Oriented Programming

### Classes and Objects

```php
<?php
class User {
    // Properties
    private $id;
    private $name;
    private $email;
    protected $createdAt;
    public $isActive;
    
    // Constructor
    public function __construct($name, $email) {
        $this->name = $name;
        $this->email = $email;
        $this->createdAt = date('Y-m-d H:i:s');
        $this->isActive = true;
    }
    
    // Getter methods
    public function getName() {
        return $this->name;
    }
    
    public function getEmail() {
        return $this->email;
    }
    
    public function getId() {
        return $this->id;
    }
    
    // Setter methods
    public function setName($name) {
        if (!empty($name)) {
            $this->name = $name;
        }
    }
    
    public function setEmail($email) {
        if (filter_var($email, FILTER_VALIDATE_EMAIL)) {
            $this->email = $email;
        }
    }
    
    // Methods
    public function getFullInfo() {
        return [
            'id' => $this->id,
            'name' => $this->name,
            'email' => $this->email,
            'created_at' => $this->createdAt,
            'is_active' => $this->isActive
        ];
    }
    
    public function deactivate() {
        $this->isActive = false;
    }
    
    // Static method
    public static function validateEmail($email) {
        return filter_var($email, FILTER_VALIDATE_EMAIL) !== false;
    }
}

// Using the class
$user = new User("John Doe", "john@example.com");
echo $user->getName();  // John Doe

$user->setName("Jane Doe");
print_r($user->getFullInfo());

// Static method call
if (User::validateEmail("test@example.com")) {
    echo "Valid email";
}
?>
```

### Inheritance

```php
<?php
class Animal {
    protected $name;
    protected $species;
    
    public function __construct($name, $species) {
        $this->name = $name;
        $this->species = $species;
    }
    
    public function makeSound() {
        return "Some generic animal sound";
    }
    
    public function getInfo() {
        return "Name: {$this->name}, Species: {$this->species}";
    }
}

class Dog extends Animal {
    private $breed;
    
    public function __construct($name, $breed) {
        parent::__construct($name, "Canine");
        $this->breed = $breed;
    }
    
    // Override parent method
    public function makeSound() {
        return "Woof! Woof!";
    }
    
    public function getBreed() {
        return $this->breed;
    }
    
    // Additional method
    public function fetch() {
        return "{$this->name} is fetching the ball!";
    }
}

class Cat extends Animal {
    public function __construct($name) {
        parent::__construct($name, "Feline");
    }
    
    public function makeSound() {
        return "Meow!";
    }
    
    public function purr() {
        return "{$this->name} is purring contentedly.";
    }
}

// Using inheritance
$dog = new Dog("Buddy", "Golden Retriever");
echo $dog->makeSound();  // Woof! Woof!
echo $dog->fetch();      // Buddy is fetching the ball!

$cat = new Cat("Whiskers");
echo $cat->makeSound();  // Meow!
echo $cat->purr();       // Whiskers is purring contentedly.
?>
```

## Database Operations

### MySQL Connection (MySQLi)

```php
<?php
// Database configuration
$servername = "localhost";
$username = "your_username";
$password = "your_password";
$dbname = "your_database";

// Create connection
$conn = new mysqli($servername, $username, $password, $dbname);

// Check connection
if ($conn->connect_error) {
    die("Connection failed: " . $conn->connect_error);
}

// Create table
$sql = "CREATE TABLE IF NOT EXISTS users (
    id INT(6) UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(50) NOT NULL,
    email VARCHAR(50) NOT NULL UNIQUE,
    age INT(3),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
)";

if ($conn->query($sql) === TRUE) {
    echo "Table users created successfully<br>";
}

// Insert data
$name = "John Doe";
$email = "john@example.com";
$age = 30;

$stmt = $conn->prepare("INSERT INTO users (name, email, age) VALUES (?, ?, ?)");
$stmt->bind_param("ssi", $name, $email, $age);

if ($stmt->execute()) {
    echo "New record created successfully<br>";
    $user_id = $conn->insert_id;
    echo "User ID: $user_id<br>";
}

// Select data
$sql = "SELECT id, name, email, age, created_at FROM users";
$result = $conn->query($sql);

if ($result->num_rows > 0) {
    echo "<h3>Users:</h3>";
    while($row = $result->fetch_assoc()) {
        echo "ID: " . $row["id"]. " - Name: " . $row["name"]. " - Email: " . $row["email"]. " - Age: " . $row["age"]. "<br>";
    }
} else {
    echo "0 results";
}

// Update data
$new_age = 31;
$user_id = 1;

$stmt = $conn->prepare("UPDATE users SET age = ? WHERE id = ?");
$stmt->bind_param("ii", $new_age, $user_id);

if ($stmt->execute()) {
    echo "Record updated successfully<br>";
}

// Delete data
$stmt = $conn->prepare("DELETE FROM users WHERE id = ?");
$stmt->bind_param("i", $user_id);

if ($stmt->execute()) {
    echo "Record deleted successfully<br>";
}

$conn->close();
?>
```

### PDO (PHP Data Objects)

```php
<?php
try {
    $pdo = new PDO("mysql:host=localhost;dbname=your_database", $username, $password);
    $pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
    
    // Insert with PDO
    $stmt = $pdo->prepare("INSERT INTO users (name, email, age) VALUES (:name, :email, :age)");
    $stmt->execute([
        ':name' => 'Jane Doe',
        ':email' => 'jane@example.com',
        ':age' => 28
    ]);
    
    echo "User inserted successfully<br>";
    
    // Select with PDO
    $stmt = $pdo->prepare("SELECT * FROM users WHERE age > :age");
    $stmt->execute([':age' => 25]);
    
    $users = $stmt->fetchAll(PDO::FETCH_ASSOC);
    
    foreach ($users as $user) {
        echo "Name: {$user['name']}, Email: {$user['email']}<br>";
    }
    
} catch(PDOException $e) {
    echo "Error: " . $e->getMessage();
}
?>
```

## File Operations

### Reading and Writing Files

```php
<?php
// Write to file
$filename = "data.txt";
$content = "Hello, World!\nThis is a test file.";

if (file_put_contents($filename, $content) !== false) {
    echo "File written successfully<br>";
}

// Read entire file
$fileContent = file_get_contents($filename);
echo "File content: " . nl2br($fileContent) . "<br>";

// Read file line by line
$handle = fopen($filename, "r");
if ($handle) {
    while (($line = fgets($handle)) !== false) {
        echo "Line: " . htmlspecialchars($line) . "<br>";
    }
    fclose($handle);
}

// Append to file
$additionalContent = "\nAppended content";
file_put_contents($filename, $additionalContent, FILE_APPEND);

// Check if file exists
if (file_exists($filename)) {
    echo "File exists<br>";
    echo "File size: " . filesize($filename) . " bytes<br>";
    echo "Last modified: " . date("Y-m-d H:i:s", filemtime($filename)) . "<br>";
}

// Delete file
if (unlink($filename)) {
    echo "File deleted successfully<br>";
}
?>
```

### File Upload

```html
<!-- upload.html -->
<!DOCTYPE html>
<html>
<head>
    <title>File Upload</title>
</head>
<body>
    <form action="upload.php" method="post" enctype="multipart/form-data">
        <label>Select file to upload:</label>
        <input type="file" name="fileToUpload" id="fileToUpload">
        <input type="submit" value="Upload File" name="submit">
    </form>
</body>
</html>
```

```php
<?php
// upload.php
if ($_SERVER['REQUEST_METHOD'] == 'POST') {
    $target_dir = "uploads/";
    $target_file = $target_dir . basename($_FILES["fileToUpload"]["name"]);
    $uploadOk = 1;
    $imageFileType = strtolower(pathinfo($target_file, PATHINFO_EXTENSION));
    
    // Check if file already exists
    if (file_exists($target_file)) {
        echo "Sorry, file already exists.";
        $uploadOk = 0;
    }
    
    // Check file size (limit to 5MB)
    if ($_FILES["fileToUpload"]["size"] > 5000000) {
        echo "Sorry, your file is too large.";
        $uploadOk = 0;
    }
    
    // Allow certain file formats
    $allowed_types = ["jpg", "png", "jpeg", "gif", "pdf", "txt"];
    if (!in_array($imageFileType, $allowed_types)) {
        echo "Sorry, only JPG, JPEG, PNG, GIF, PDF & TXT files are allowed.";
        $uploadOk = 0;
    }
    
    // Check if $uploadOk is set to 0 by an error
    if ($uploadOk == 0) {
        echo "Sorry, your file was not uploaded.";
    } else {
        // Create uploads directory if it doesn't exist
        if (!file_exists($target_dir)) {
            mkdir($target_dir, 0777, true);
        }
        
        if (move_uploaded_file($_FILES["fileToUpload"]["tmp_name"], $target_file)) {
            echo "The file ". htmlspecialchars(basename($_FILES["fileToUpload"]["name"])). " has been uploaded.";
        } else {
            echo "Sorry, there was an error uploading your file.";
        }
    }
}
?>
```

## Sessions and Cookies

### Sessions

```php
<?php
// Start session
session_start();

// Set session variables
$_SESSION["username"] = "john_doe";
$_SESSION["user_id"] = 123;
$_SESSION["role"] = "admin";

echo "Session variables set.<br>";

// Access session variables
if (isset($_SESSION["username"])) {
    echo "Welcome, " . $_SESSION["username"] . "!<br>";
}

// Check if user is logged in
function isLoggedIn() {
    return isset($_SESSION["user_id"]);
}

if (isLoggedIn()) {
    echo "User is logged in<br>";
} else {
    echo "User is not logged in<br>";
}

// Destroy specific session variable
unset($_SESSION["role"]);

// Destroy all session variables
// session_destroy();
?>
```

### Cookies

```php
<?php
// Set cookie (expires in 1 hour)
$cookie_name = "user_preference";
$cookie_value = "dark_theme";
$expire_time = time() + (60 * 60); // 1 hour

setcookie($cookie_name, $cookie_value, $expire_time, "/");

echo "Cookie set successfully<br>";

// Read cookie
if (isset($_COOKIE[$cookie_name])) {
    echo "Cookie value: " . $_COOKIE[$cookie_name] . "<br>";
} else {
    echo "Cookie not found<br>";
}

// Delete cookie (set expiration to past time)
// setcookie($cookie_name, "", time() - 3600, "/");
?>
```

## Error Handling

### Basic Error Handling

```php
<?php
// Enable error reporting for development
error_reporting(E_ALL);
ini_set('display_errors', 1);

// Custom error handler
function customErrorHandler($errno, $errstr, $errfile, $errline) {
    echo "<b>Error:</b> [$errno] $errstr<br>";
    echo "Error on line $errline in $errfile<br>";
}

// Set custom error handler
set_error_handler("customErrorHandler");

// Try-catch for exceptions
try {
    $pdo = new PDO("mysql:host=localhost;dbname=nonexistent", "user", "pass");
} catch (PDOException $e) {
    echo "Database connection failed: " . $e->getMessage() . "<br>";
}

// Custom exception
class CustomException extends Exception {
    public function errorMessage() {
        return "Error on line {$this->getLine()} in {$this->getFile()}: {$this->getMessage()}";
    }
}

function validateAge($age) {
    if ($age < 0) {
        throw new CustomException("Age cannot be negative");
    }
    if ($age > 150) {
        throw new CustomException("Age seems unrealistic");
    }
    return true;
}

try {
    validateAge(-5);
} catch (CustomException $e) {
    echo $e->errorMessage() . "<br>";
}
?>
```

## Best Practices

### Security

```php
<?php
// Input validation and sanitization
function sanitizeInput($data) {
    $data = trim($data);
    $data = stripslashes($data);
    $data = htmlspecialchars($data);
    return $data;
}

// Password hashing
$password = "user_password";
$hashed_password = password_hash($password, PASSWORD_DEFAULT);

// Verify password
if (password_verify($password, $hashed_password)) {
    echo "Password is valid<br>";
}

// Prevent SQL injection with prepared statements
$stmt = $pdo->prepare("SELECT * FROM users WHERE email = ? AND password = ?");
$stmt->execute([$email, $hashed_password]);

// CSRF protection
session_start();
if (!isset($_SESSION['csrf_token'])) {
    $_SESSION['csrf_token'] = bin2hex(random_bytes(32));
}

// In forms, include:
// <input type="hidden" name="csrf_token" value="<?php echo $_SESSION['csrf_token']; ?>">

// Validate CSRF token
if ($_POST['csrf_token'] !== $_SESSION['csrf_token']) {
    die('CSRF token mismatch');
}
?>
```

### Code Organization

```php
<?php
// config.php
define('DB_HOST', 'localhost');
define('DB_USER', 'username');
define('DB_PASS', 'password');
define('DB_NAME', 'database');

// Database connection class
class Database {
    private $pdo;
    
    public function __construct() {
        try {
            $this->pdo = new PDO(
                "mysql:host=" . DB_HOST . ";dbname=" . DB_NAME,
                DB_USER,
                DB_PASS
            );
            $this->pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
        } catch (PDOException $e) {
            die("Database connection failed: " . $e->getMessage());
        }
    }
    
    public function query($sql, $params = []) {
        $stmt = $this->pdo->prepare($sql);
        $stmt->execute($params);
        return $stmt;
    }
    
    public function fetch($sql, $params = []) {
        return $this->query($sql, $params)->fetch(PDO::FETCH_ASSOC);
    }
    
    public function fetchAll($sql, $params = []) {
        return $this->query($sql, $params)->fetchAll(PDO::FETCH_ASSOC);
    }
}

// User class
class User {
    private $db;
    
    public function __construct(Database $db) {
        $this->db = $db;
    }
    
    public function create($name, $email, $password) {
        $hashed_password = password_hash($password, PASSWORD_DEFAULT);
        
        return $this->db->query(
            "INSERT INTO users (name, email, password) VALUES (?, ?, ?)",
            [$name, $email, $hashed_password]
        );
    }
    
    public function findByEmail($email) {
        return $this->db->fetch(
            "SELECT * FROM users WHERE email = ?",
            [$email]
        );
    }
    
    public function authenticate($email, $password) {
        $user = $this->findByEmail($email);
        
        if ($user && password_verify($password, $user['password'])) {
            return $user;
        }
        
        return false;
    }
}

// Usage
$db = new Database();
$userModel = new User($db);

// Create user
$userModel->create("John Doe", "john@example.com", "secure_password");

// Authenticate user
$user = $userModel->authenticate("john@example.com", "secure_password");
if ($user) {
    echo "Authentication successful!";
}
?>
```

## Next Steps

After mastering PHP basics:

1. **Learn Laravel Framework**: Modern PHP framework for web applications
2. **Study Composer**: PHP dependency manager
3. **Explore APIs**: Build RESTful APIs with PHP
4. **Master Testing**: PHPUnit for testing PHP applications
5. **Learn Advanced OOP**: Traits, namespaces, and design patterns

PHP's simplicity and extensive ecosystem make it an excellent choice for web development. Its integration with databases and web servers, combined with frameworks like Laravel, enables rapid development of robust web applications.

Remember to always follow security best practices, validate user input, and keep your PHP version updated for the latest features and security improvements.
