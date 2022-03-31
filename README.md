# CS568-Assign4-ListsConditionals
- Conditionally show/hide the Todo list.
- Add a field that inserts the task in to the todo list.
- Add "edit todo" button. 

```
App (root component)
├─ AddTodo
├─ TodoList
│  └─ TodoItem
│     ├─ TodoDeleteButton
│     ├─ TodoEditButton
└─ TodoFooter
```

- Make the todo item a JSON object. `{id,task,isDeleted,createdAt}`. 
- Pass the id to delete the component (change isDeleted flag from 0 to 1) and hide the deleted ones from the list. It is known as soft-delete.
- Show all attributes in the todo item component.
- Update the add and delete functions accordingly.
