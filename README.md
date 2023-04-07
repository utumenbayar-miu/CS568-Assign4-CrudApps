# CS568 - Assignment 4 - CRUD apps

This assignment is for two day classes, conidtionals and lifecycle.
On first day, do the following:
* READ (LIST)
* CREATE
* DELETE
* CONDITIONALS and LIST

Skip update and lifecycle (useEffect) for today. Do them tomorrow.

> Please do the add and update in the same form. So you can practice state more. In order to share state between components, you may need to lift it up to the parent component.

Main concepts we practice in this assignment:
* Lift and shift state
* Passing a function as a prop for data binding
* How to get data from form
* Conditionals, list, key
* State immutability

## Main tasks
1. Implement the CRUD operations for the sample **Students** app. Don't copy and past the code below. Instead, try to understand. Feel free to come up with your own CRUD logic.
2. Implement the same tasks, CRUD operations for the **TodoList** app. Style it.

## Microtask
* Play with the useEffect hook. There are 3 alternatives in useEffect, componentDidMount, componentDidUpdate, and componentWillUnmount. You may want to comment out the `<React.StrictMode>` in the index.js. Otherwise, in dev mode, React calls your component multiple times to detect problems.
* Practice `React.memo`.

## Extra
* When there are multiple operations on a single state like addStudent, updateStudent, deleteStudent on students state, you can use `useReducer` which is better practice. Replace `useState` with `useReducer`.

## Refer
### Component lifecycle methods
```
import { useEffect } from "react";
import { useState } from "react";
import "./App.css";

function App() {
  const [isHidden, setisHidden] = useState(false);
  return (
    <div className="App">
      {!isHidden && <MyComponent />}
      <button onClick={() => setisHidden(!isHidden)}>hide/show</button>
    </div>
  );
}

function MyComponent() {
  const [cntr, setcntr] = useState(0);
  const [cntr2, setcntr2] = useState(0);
  
  useEffect(() => {
    console.log("Component is mounted to the DOM!!");
    // This is where you make an API call to the server (componentDidMount).
    return () => {
      // It is componentWillUnmount.
      // Your logic when component is removed such as unsubscribe streaming data from the server.
      console.log("Component is unmounted or removed from the DOM");
    };
  }, []);

  // componentDidUpdate
  useEffect(() => {
    console.log("It is triggered every time the component is updated");
    // if you pass a state in the dependency array, it will only get triggered when that state is changed. 
  }, [cntr]);

  return (
    <p>
      MyComponent, counter: {cntr}, counter 2: {cntr2},{" "}
      <button
        onClick={() => {
          setcntr(cntr + 1);
        }}
      >
        increment
      </button>
      <button
        onClick={() => {
          setcntr2(cntr2 + 1);
        }}
      >
        increment 2
      </button>
    </p>
  );
}
export default App;
```
### React.memo
```
import { memo, useState } from "react";
import "./App.css";

function App() {
  const [cntr, setcntr] = useState(0);

  return (
    <div className="App">
      <button onClick={() => setcntr(cntr + 1)}>{cntr}</button>
      <HelloWithMemo name="Uno"/>
    </div>
  );
}

function Hello({name}) {
  console.log("invoked");
  return <p>hello {name}</p>
}

const HelloWithMemo = memo(function Hello({name}) {
  console.log("invoked");
  return <p>hello {name}</p>
});

export default App;
```
### Main code
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
