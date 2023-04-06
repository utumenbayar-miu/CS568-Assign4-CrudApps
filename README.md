# CS568 - Assignment 4 - CRUD apps

## Main tasks
1. Implement the CRUD operations for the sample **Students** app. Don't copy and past the code below. Instead, try to understand. Feel free to come up with your own CRUD logic.
2. Implement the same tasks, CRUD operations for the **TodoList** app. Style it.

## Microtask
* Play with the useEffect hook. There are 3 alternatives in useEffect, componentDidMount, componentDidUpdate, and componentWillUnmount.

Refer:
```
import { useEffect, useState } from "react";
import "./App.css";
import { v4 as uuidv4 } from "uuid";

function App() {
  const [students, setStudents] = useState([
    { id: 1234, name: "s1", age: 12 },
    { id: 12, name: "s2", age: 10 },
    { id: 32, name: "s3", age: 11 },
  ]);
  const [whichForm, setWhichForm] = useState("add");
  const [studentToUpdate, setStudentToUpdate] = useState({ name: "", age: 0 });

  function addStudent(student) {
    const newStudents = [...students];
    newStudents.unshift(student);
    setStudents(newStudents);
  }

  function updateStudent(student) {
    const newStudents = [...students];

    for (let s of newStudents) {
      if (s.id === student.id) {
        s.name = student.name;
        s.age = student.age;
        break;
      }
    }

    setStudents(newStudents);
  }

  function deleteStudent(studentId) {
    setStudents(students.filter((s) => (s.id !== studentId ? s : null)));
  }

  return (
    <div className="App">
      <h2>Student List</h2>
      {students.map((s) => (
        <Student
          key={s.id}
          id={s.id}
          name={s.name}
          age={s.age}
          deleteStudent={deleteStudent}
          setWhichForm={setWhichForm}
          setStudentToUpdate={setStudentToUpdate}
        />
      ))}
      <StudentForm
        addStudent={addStudent}
        whichForm={whichForm}
        studentToUpdate={studentToUpdate}
        updateStudent={updateStudent}
        setWhichForm={setWhichForm}
      />
    </div>
  );
}

function Student({
  id,
  name,
  age,
  deleteStudent,
  setWhichForm,
  setStudentToUpdate,
}) {
  return (
    <p>
      student id: {id}, name: {name}, age: {age}&nbsp;
      <button onClick={() => deleteStudent(id)}>delete</button>
      <button
        onClick={() => {
          setWhichForm("update");
          setStudentToUpdate({ id, name, age });
        }}
      >
        update
      </button>
    </p>
  );
}

function StudentForm({
  addStudent,
  whichForm,
  studentToUpdate,
  updateStudent,
  setWhichForm
}) {
  const [name, setName] = useState(studentToUpdate.name);
  const [age, setAge] = useState(studentToUpdate.age);

  useEffect(() => {
    setName(studentToUpdate.name);
    setAge(studentToUpdate.age);
  }, [studentToUpdate])

  function cleanForm() {
    setName("");
    setAge(0);
  }

  return (
    <>
      <h2>{whichForm === "add" ? "Add a student" : "Update a student"}</h2>
      {whichForm === "update" && <button onClick={() => {
        setWhichForm("add");
        cleanForm();
      }}>Change to add a student</button>}
      <input value={name} onChange={(e) => setName(e.target.value)} />
      <input
        type="number"
        value={age}
        onChange={(e) => setAge(e.target.value)}
      />
      <button
        onClick={() => {
          const student = {
            name,
            age,
          };
          if (whichForm === "add") {
            student.id = uuidv4();
            addStudent(student);
            cleanForm();
          } else {
            student.id = studentToUpdate.id;
            updateStudent(student);
            cleanForm();
          }
        }}
      >
        Submit
      </button>
    </>
  );
}

export default App;

```
