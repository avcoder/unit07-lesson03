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
layout: image-right
transition: slide-left
image: /assets/addy.png
backgroundSize: 400px 300px
class: text-left
---

# 10 minute break

🍦 Cool Tips, Trends and Resources:
- 👩‍💻 [NeetCode](https://www.freecodecamp.org/news/prepare-for-technical-interviews-using-neetcode-150)

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