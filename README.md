# CS568 - Assignment 4 - Lists and Conditionals
- Conditionally show/hide the Todo list.
- Add a field that inserts the task in to the todo list.

```
App (root component)
├─ AddTodo
├─ TodoList
│  └─ TodoItem
│     ├─ TodoDeleteButton
└─ TodoFooter
```

- Make the todo item a JSON object. `{id,task,username,isDeleted}`. 
- Pass the id to delete the component (change isDeleted flag from 0 to 1) and hide the deleted ones from the list. It is known as soft-delete.
- Show all attributes except isDeleted in the todo item component.
- Update the add function accordingly. Add the username field. You can use UUID node package (recommended) or you can take the id via text input.

BONUS TASK +2
- Implement "edit todo". 
