<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Student Registration</title>

<style>
body{
    font-family: Arial, sans-serif;
    background:#f4f6fb;
    padding:20px;
    scroll-behavior:smooth;
}
.container{
    max-width:600px;
    background:white;
    padding:20px;
    border-radius:10px;
    margin:auto;
    box-shadow:0 4px 10px rgba(0,0,0,0.1);
}
h2{text-align:center;}
.input-group{margin-bottom:15px;}
label{font-weight:bold;}
input,select{
    width:100%;
    padding:10px;
    margin-top:5px;
    border-radius:5px;
    border:1px solid #ccc;
}
.radio-group{display:flex;gap:20px;margin-top:5px;}
button{
    width:100%;
    padding:12px;
    background:#435ebe;
    color:white;
    border:none;
    border-radius:5px;
    font-size:16px;
    cursor:pointer;
}
button:hover{background:#2f46a3;}

table{
    width:100%;
    margin-top:40px;
    border-collapse:collapse;
    background:white;
}
th{
    background:#5a73c7;
    color:white;
    padding:10px;
}
td{
    padding:10px;
    text-align:center;
    border-bottom:1px solid #ddd;
}

.btn-edit{
    background:#ffc107;
    border:none;
    padding:5px 8px;
    border-radius:4px;
    cursor:pointer;
}
.btn-delete{
    background:#dc3545;
    color:white;
    border:none;
    padding:5px 8px;
    border-radius:4px;
    cursor:pointer;
}
</style>
</head>

<body>

<div class="container">
<h2>Student Registration</h2>

<div class="input-group">
<label>Name</label>
<input type="text" id="name">
</div>

<div class="input-group">
<label>Email</label>
<input type="email" id="email">
</div>

<div class="input-group">
<label>Age</label>
<input type="number" id="age">
</div>

<div class="input-group">
<label>Gender</label>
<div class="radio-group">
<input type="radio" name="gender" value="Male"> Male
<input type="radio" name="gender" value="Female"> Female
</div>
</div>

<div class="input-group">
<label>Course</label>
<select id="course">
<option>Java Full Stack</option>
<option>Python Full Stack</option>
<option>Data Science</option>
</select>
</div>

<div class="input-group">
<label>Date</label>
<input type="date" id="regDate">
</div>

<button onclick="registerStudent()">Register</button>
</div>

<!-- STORE SECTION -->
<table id="storeSection">
<thead>
<tr>
<th>Name</th>
<th>Email</th>
<th>Age</th>
<th>Gender</th>
<th>Course</th>
<th>Date</th>
<th>Action</th>
</tr>
</thead>
<tbody id="dataTable"></tbody>
</table>

<script>
let students = JSON.parse(localStorage.getItem("students")) || [];
let editIndex = -1;

window.onload = displayStudents;

function registerStudent(){
    const name = document.getElementById("name").value;
    const email = document.getElementById("email").value;
    const age = document.getElementById("age").value;
    const genderEl = document.querySelector('input[name="gender"]:checked');
    const course = document.getElementById("course").value;
    const regDate = document.getElementById("regDate").value;

    if(!name || !email || !age || !genderEl || !regDate){
        alert("Ellaa details-um fill pannunga");
        return;
    }

    const student = {
        name,
        email,
        age,
        gender: genderEl.value,
        course,
        regDate
    };

    if(editIndex === -1){
        students.push(student);
    alert("Registration Successfully ✅");
    }else{
        students[editIndex] = student;
        editIndex = -1;
      alert("Updated Successfully ✏️");
    }

    localStorage.setItem("students", JSON.stringify(students));
    displayStudents();
    clearForm();
    setTodayDate();

    document.getElementById("storeSection").scrollIntoView();
}

function displayStudents(){
    const table = document.getElementById("dataTable");
    table.innerHTML = "";

    students.forEach((s,i)=>{
        const row = `
        <tr>
            <td>${s.name}</td>
            <td>${s.email}</td>
            <td>${s.age}</td>
            <td>${s.gender}</td>
            <td>${s.course}</td>
            <td>${s.regDate}</td>
            <td>
                <button class="btn-edit" onclick="editStudent(${i})">Edit</button>
                <button class="btn-delete" onclick="deleteStudent(${i})">Delete</button>
            </td>
        </tr>`;
        table.innerHTML += row;
    });
}

function deleteStudent(i){
    if(confirm("Delete panna sure-aa?")){
        students.splice(i,1);
        localStorage.setItem("students",JSON.stringify(students));
        displayStudents();
    }
}

function editStudent(i){
    const s = students[i];
    document.getElementById("name").value = s.name;
    document.getElementById("email").value = s.email;
    document.getElementById("age").value = s.age;
    document.getElementById("course").value = s.course;
    document.getElementById("regDate").value = s.regDate;
    document.querySelector(`input[value="${s.gender}"]`).checked = true;
    editIndex = i;
}

function clearForm(){
    document.getElementById("name").value="";
    document.getElementById("email").value="";
    document.getElementById("age").value="";
    document.getElementById("regDate").value="";
    const g=document.querySelector('input[name="gender"]:checked');
    if(g) g.checked=false;
}
</script>

</body>
</html>
