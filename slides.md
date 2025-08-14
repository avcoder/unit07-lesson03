---
# You can also start simply with 'default'
theme: default
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: /assets/intro.jpg
# some information about your slides (markdown enabled)
title: Software Development | Foundations
info: |
  ## Software Development | Foundations
# apply unocss classes to the current slide
class: text-left
drawings:
  persist: false
transition: slide-left
mdc: true
---

# Intro to Deployment
Unit 07 - Lesson 03

- [ ] PHP
- [ ] Connect to DB
- [ ] Display a data table from mySQL 

<div class="abs-br m-6 text-xl">
  <a href="https://github.com/slidevjs/slidev" target="_blank" class="slidev-icon-btn">
    <carbon:logo-github />
  </a>
</div>

<!--
-->

---
transition: slide-left
---

# Create first PHP page

- Start PHP and mySQL server in XAMPP
- In windows: `C:/XAMPP/htdocs/`
- In MAC: Finder > search XAMPP > most likely in `/Applications/XAMPP/htdocs/`

## Exercise
- create `/unit07/index.php`
```php
<?php echo "Hello world" ?>
```
- goto localhost/unit07/index.php
- What happens if you didn't specify `index.php`?

---
transition: slide-left
---

# Connect to DB locally

- create `/unit07/db.php`
```php
<?php
  $conn = new PDO('mysql:host=localhost;dbname=games', 'root', '');
  echo 'connected to db';
?>
```




---
transition: slide-left
---

# Extract form values from GET vs POST requests

- create login.php
```html
<fosrm action="validate.php">
  <input type="text" name="username">
  <input type="password" name="password">
  <button>Submit</button>
</form>
```
- create validate.php
```php
<?php

$username = $_POST['username'];
$password = $_POST['password'];

echo $username;
echo $password;

?>
```
- try the above using a GET request, then a `method="POST"` request

---
transition: slide-left
---

# Exercise (pg.1)
Create mySQL table to save games data 

```sql
USE INSERT_DB_NAME_HERE;

DROP TABLE IF EXISTS games;

CREATE TABLE IF NOT EXISTS games (
  game_id INT NOT NULL AUTO_INCREMENT,
  title VARCHAR(50),
  year INT,
  genre VARCHAR(32),
  url VARCHAR(100),
  PRIMARY KEY (game_id));

INSERT INTO games (title, year, genre, url) VALUES
('Pacman',1983,'Puzzle','https://archive.org/details/PACMAN_CGA_2'),
('Dig Dug', 1983, 'Puzzle', 'https://archive.org/details/DIGDUG_CGA'),
('Jeopardy', 1991, 'Trivia', 'https://archive.org/details/jeopardy-25th-anniversary-edition');
```

---
transition: slide-left
---

# Exercise (pg.2)
Create an `add-game.php` that uses an input form to capture new data

- Tip: use ! in VS Code to scaffold an HTML page
```html
<form action="" method="GET or POST?">
  <label for="title">title</label>
  <input type="text" name="title">
  <label for="year">year</label>
  <input type="text" name="year">
  <label for="genre">genre</label>
  <input type="text" name="genre"> <!-- refactor to use dropdown with options: 'puzzle' 'trivia'-->
  <label for="url">url</label>
  <input type="text" name="url">
  <button>Submit</button>
</form>
```
- Optionally can add basic styles: https://watercss.kognise.dev/

---
transition: slide-left
---

# Exercise (pg.3)

- Refactor header/footer into partials header.php and footer.php
```php
require_once 'components/header.php'; 
// OR can also include_once 'components/header.php' but this won't error out if it doesn't exist
```
- Optionally: Create a homepage `home.php` that includes header.php and footer.php and some game image


---
transition: slide-left
---

# Exercise (pg.4)
Create save-game.php

```php
<?php
$title = // FILL-IN-THE-BLANK: save all 4 form input into variables
$year = 
$genre = 
$url = 

require 'db.php'; // connect to DB

// set up SQL INSERT command
$sql = "INSERT INTO games (title, year, genre, url) VALUES (:title, :year, :genre, :url)";

// create a command object and fill the parameters with the form values
$cmd = $conn->prepare($sql);
$cmd -> bindParam(':title', $title, PDO::PARAM_STR, 50);
$cmd -> bindParam(':year', $title, PDO::PARAM_INT);
$cmd -> bindParam(':genre', $title, PDO::PARAM_STR, 32);
$cmd -> bindParam(':url', $title, PDO::PARAM_STR, 100);

// execute the command, then disconnect db, then show msg
$cmd -> execute();
$conn = null;
echo "Game saved?";
?>
```

---
transition: slide-left
---

# Fetch data from mySQL and display table

```php
// connect to db
require_once 'db.php';

// build sql query
$sql = "SELECT * FROM games"

// run query and store results
$cmd = $conn -> prepare($sql);
$cmd -> execute();
$games = $cmd->fetchAll(); 
// Try: 1) echo $games 2) var_dump($games) 3) print_r($games)
// 4) echo json_encode($games) 5) echo json_encode($games[2]) 6) echo json_encode($games[2]['title'])

// show db results
echo "<table>"
foreach ($games as $game) {
  echo "<tr><td>" . $game['title'] . "</td></tr>";
}
echo "</table>"

// disconnect
$conn = null;
```
- ChatGPT: Refactor this PHP code to output JSON to make an API endpoint and avoid CORS errors?

---
transition: slide-left
---

# Exercise

- Create table that displays all data (all 4 columns)
- Create additional Columns for 'Edit', 'Delete'

---
transition: slide-left
---

# Functions

- create db-queries.php, then move below function into it
```php
function db_queryAll($sql, $conn) {
  $cmd = $conn -> prepare($sql);
  $cmd -> execute();
  $games = $cmd -> fetchAll();

  return $games;
}
```
- can now do this:
```php
require_once 'db-queries.php';
...
$sql = "SELECT * FROM games";
// now can do this
$games = db_queryAll($sql, $conn);
```

---
transition: slide-left
---

# Exercise
Create a function to refactor our database connections

- Refactor our database connection so we can use it more elegantly like:
`$conn = db_connect()`
```php
// delete require 'db.php';
// in db-queries.php create below function
function db_connect() {
  $conn = new PDO(....)
  return $conn;
}
```
- Refactor our disconnect to be inside footer.php via a function you should create called `db_disconnect()`
```php
function db_disconnect($conn) {
  if (isset($conn)) {
    $conn = null;
  }
}
```

---
transition: slide-left
---

# Define Constants

- create db_cred.php
```php
define("DB_SERVER", "localhost");
define("DB_NAME", "insert-db-name-here");
define("DB_USERNAME", "root");
define("DB_PASSWORD", "");
```
- which allows you to use it via:
```php
require_once 'db_cred.php';

function db_connect() {
  $conn = new PDO('mysql:host=' . DB_SERVER . ';dbname=' . DB_NAME, DB_USERNAME, DB_PASSWORD)
}
```

# Exercise

- Implement a dynamic dropdown menu by first creating a new database table that will store the genres
- Edit your form page so that the dropdown menu no longer is hard-coded with HTML but rather will use PHP to fetch from your new table dynamically. 

---
layout: image-right
transition: slide-left
image: /assets/jeff.png
backgroundSize: 400px 370px
class: text-left
---

# 10 minute break

🍦 Cool Tips, Trends and Resources:
- 🧵 [Intro to SVG](https://www.joshwcomeau.com/svg/friendly-introduction-to-svg/)
- 🏰 [Laracasts](https://laracasts.com/)

<br>
<hr>
<br>

- 🧪 [Enter anonymous lab questions](https://docs.google.com/forms/d/e/1FAIpQLSevvGARdHQikso-uLqFCO481MABKE5HofuSrlzEPMNQ2ZLykw/viewform?usp=dialog)
- ℹ️ [Course feedback survey](https://circuitstream.typeform.com/to/ZoyYk7px#course_id=SoftwareAN&instructor=9514)

---
transition: slide-left
---

# Homework

- Work on your "Weather App" assignment due Aug 17 midnight EST
   - can submit before due date if you wish