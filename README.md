<script>

let students = [];

function addStudent() {

    const roll = document.getElementById("rollNo").value.trim();
    const name = document.getElementById("studentName").value.trim();

    if (roll === "" || name === "") {
        alert("Please enter Roll Number and Student Name");
        return;
    }

    students.push({
        roll: roll,
        name: name,
        present: 0,
        absent: 0
    });

    document.getElementById("rollNo").value = "";
    document.getElementById("studentName").value = "";

    displayStudents();
}

function markPresent(index) {
    students[index].present++;
    displayStudents();
}

function markAbsent(index) {
    students[index].absent++;
    displayStudents();
}

function deleteStudent(index) {
    students.splice(index, 1);
    displayStudents();
}

function displayStudents() {

    const table = document.getElementById("studentTable");
    const search = document.getElementById("search").value.toLowerCase();

    table.innerHTML = "";

    let filteredStudents = students.filter(student =>
        student.roll.toLowerCase().includes(search) ||
        student.name.toLowerCase().includes(search)
    );

    if (filteredStudents.length === 0) {
        table.innerHTML = `
            <tr>
                <td colspan="6" class="no-data">
                    No students found
                </td>
            </tr>
        `;
    }

    filteredStudents.forEach(student => {

        const index = students.indexOf(student);

        const total = student.present + student.absent;

        const percentage = total === 0
            ? 0
            : ((student.present / total) * 100).toFixed(1);

        table.innerHTML += `
            <tr>
                <td>${student.roll}</td>

                <td>${student.name}</td>

                <td>
                    <button class="present"
                        onclick="markPresent(${index})">
                        + Present
                    </button>
                    <br>
                    ${student.present}
                </td>

                <td>
                    <button class="absent"
                        onclick="markAbsent(${index})">
                        + Absent
                    </button>
                    <br>
                    ${student.absent}
                </td>

                <td>
                    ${percentage}%
                </td>

                <td>
                    <button class="delete"
                        onclick="deleteStudent(${index})">
                        Delete
                    </button>
                </td>
            </tr>
        `;
    });

    document.getElementById("totalStudents").innerText =
        students.length;

    document.getElementById("totalPresent").innerText =
        students.reduce((sum, s) => sum + s.present, 0);

    document.getElementById("totalAbsent").innerText =
        students.reduce((sum, s) => sum + s.absent, 0);
}

displayStudents();

</script>
