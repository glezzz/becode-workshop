# Android Todo list app

## Requirements

-IntelliJ IDEA or Android Studio

## Getting Started
- Create an empty Android project
- Give it a name
- Select language Kotlin
- Select a Minimum SDK compatible with most of the devices

Let's get started!

If you get stuck during the exercise, Android documentation can be of great help. https://developer.android.com/docs

### Steps
1. Go to <code>acitivity_main.xml</code>. This where all your design will happen. You will be familiar with it if you attended Cis’ workshop. If you take a look at the palette you’ll see ‘RecyclerView’. Drag and drop into the screen. That will be where your items will show up later. (Small tip: if you click on the ‘Split’ option, you’ll see the xml code and the layout side by side)

    - The first block of code you see corresponds to the ‘ConstraintLayout’, that’s the screen itself. The second block is for the ‘RecyclerView’ you just dropped in there. Let’s make some changes to that code.

2. Change the width to “match_parent” and the height to “wrap_content” for now.

3. Now if you look at the layout you’ll notice some blue circles to every side of the component. Play around with them and see if you can manage to get the list of items to stick to the top left of the screen. (‘Ctrl + Right click’ to undo constraint). Notice that code gets automatically added to the block when you set a constraint.

Let’s give the <code>RecyclerView</code> an ID so we can refer to it later on in the code. 
4. Add android:id=”@+id/rvTodoitems” to the top of the <code>RecyclerView</code> code. That’ll be all for now for this block of code. 
5. If we want to add an item to the list we’ll need a text field to enter some text. You need to add a new tag for that, let’ see if you can figure out what it is. When you do, give it “wrap_content” as width and height for now. Give it an ID too, “@+id/etTodoTitle”.
6. Now lets add hint text. That text will help the user to know what they need to enter. Let me give a hint on how to do it: hint :).
7. Add the text you wish. If you get some errors, make sure that you need to add those constraints again for this component. Let’s say to the bottom left of the screen. 
8. Now you need two buttons. I’m pretty sure you know how to add a button by now. Give it width & height “wrap_content”, ID of “@+id/btnAddTodo” and text “Add Todo” or whatever you want. Don’t set the constraints of this button yet.
9. Add the second button first by copy-pasting the code and change the ID to “@+id/btnDeleteDoneTodos” and the text to “Delete done”.
10. Now you can set the constraint of this button to the parent right, the bottom to the parent bottom. For the first button, you set the right constraint to the left one of the second button. 

You’ll notice a white gap is left between ‘Enter todo’ and ‘Add todo’. Let’s fix that.

11. Go to the <code>EditText</code> tag you added and change the width to “0dp”. That’ll fill out the whole width.
12. Set the bottom constraint of the <code>RecyclerView</code> to one of the other components, it doesn’t matter which one. You’ll notice that it gets totally centered. It has something to do with the height. Let’s see if you can figure it out.
    
Okay, now let’s set up how each individual item will look like when one is added to the list.
13. On the <code>layout</code> folder, create a new <code>Layout Resource File</code>.
14. Give it a name like ‘item_todo’ and OK. A new screen like the one before will appear.
15. Add a <code>TextView</code> tag to the code with properties “@+id/tvTodoTitle”, “0dp”, “wrap_content”, add “Example” as text and set the constraint to left, top and bottom.

Notice that the component gets centered to the middle of the screen. You need to set up the <code>ConstraintLayout</code> height to something like “80dp”. Set it however you like, you’ll see the result immediately.
16. Now you want to add a checkbox next to the example. Can you figure out how to? Don’t forget to give it an ID and set the constraints right for both components.
17. See if you can change the text size of the text view and make it a little bigger, to 24sp or so.
18. If you don’t like that the text is completely aligned to the left margin, you can add padding to it. Where do you think you should add that?

Okay, well done, you can catch a breath now. Now that the layout is done, let's start with the logic:

1. Go to the directory where your MainActivity is and create a new Kotlin Class.
2. Make a <code>data</code> class out of it. We’ll use this class as the constructor.
3. As parameters pass in a variable called title of type String and a variable <code>isChecked</code> of type boolean with a default value of false.
4. Now you need to create a so-called <code>RecycleView</code> adapter, which is basically a class in which we define the layout we want to use, in our case only one.
5. Create another Class in the same directory called <code>TodoAdapter</code>.
6. For this constructor we pass in a private variable ‘todos’, which will be a  MutableList of type Todo.
7. Inside the scope create a class called <code>TodoViewHolder</code> , this class will hold the layout of a specific item. We reference that with <code>itemView: View</code>.
8. This class will inherit from <code>RecyclerView.ViewHolder(itemView)</code>. 
9. Now go back to the <code>TodoAdapter</code> class. We need to define that this class defines such <code>RecyclerViewAdapter</code>. 
10. You do that by letting this class inherit from <code>RecyclerView.Adapter<TodoAdapter.TodoViewholder>()</code>. 

Alright, now is when it gets real spicy.

11. Under the last class create some white space and press Ctrl + i. A window will pop up with all the functions we need to implement to make it a complete <code>RecyclerViewAdapter</code>.
12. Press Ctrl + a and OK.
13. This code should go inside <code>onCreateViewHolder</code>:
```
    return TodoViewHolder(
        LayoutInflater.from(parent.context).inflate(
            R.layout.item_todo,
            parent,
            false
        )
    )
    
```
    
    
This function will create the <code>TodoViewHolder</code> and the code just describes how the view should look like. 

Now you need to create the function that will strike the text through when an item is checked.
14. Create a private function under <code>onCreateViewHolder</code>.
15. As parameters pass in the ID we gave it previously, <code>tvTodoTitle: Textview</code> and the boolean <code>isChecked</code>, we created before.
16. This should go inside the function:

```
if (isChecked) {
    tvTodoTitle.paintFlags = tvTodoTitle.paintFlags or STRIKE_THRU_TEXT_FLAG

} else {
    tvTodoTitle.paintFlags = tvTodoTitle.paintFlags and STRIKE_THRU_TEXT_FLAG.inv
}

  ```

Let’s work now on the <code>onBindViewHolder</code> function. This will be called when a new item in our list is visible. 
17. Place this code inside it: 
    
```
    val curTodo = todos[position]
    holder.itemView.apply {
        tvTodoTitle.text = curTodo.title
        cbDone.isChecked = curTodo.isChecked
        toggleStrikeThru(tvTodoTitle, curTodo.isChecked)
        cbDone.setOnCheckedChangeListener { _, isChecked ->
            toggleStrikeThru(tvTodoTitle, isChecked)
            curTodo.isChecked = !curTodo.isChecked
        }
    }
```

18. The last function is an easy one. This one should return the amount of items we have in our list.
19. You need to add a couple more though. One for adding a todo and one for deleting all done todos. So go ahead and add a function that you can call addTodo, passing in todo. Here you need to add that todo to all todos. You can use a method for that. Then we need to notify that we inserted an item. Take a look here:
    https://developer.android.com/reference/kotlin/androidx/recyclerview/widget/RecyclerView.Adapter#notifyiteminserted. 
20. The next one should delete all done todos. For the <code>deleteDoneTodos</code> function you can use this code:

```
todos.removeAll { todo ->
   todo.isChecked
}
notifyDataSetChanged()
```

Okay, stay with me, we are almost done.

21. Go to <code>MainActivity</code> and let’s define the behavior of your app.
22. Define a global private <code>lateinit</code> variable called todoAdapter.
23. We’ll initialize it inside the onCreate function by adding this code inside the <code>onCreate</code> function:
```
todoAdapter = TodoAdapter(mutableListOf())

rvTodoItems.adapter = todoAdapter
rvTodoItems.layoutManager = LinearLayoutManager(this)

btnAddTodo.setOnClickListener {
   val todoTitle = etTodoTitle.text.toString()
   if(todoTitle.isNotEmpty()) {
       val todo = Todo(todoTitle)
       todoAdapter.addTodo(todo)
       etTodoTitle.text.clear()
   }
}
btnDeleteDoneTodos.setOnClickListener {
   todoAdapter.deleteDoneTodos()
}
```

24. Congratulations! Very well done! You made it! Now test it out and see if your app works. After that you can work on design more and experiment with it.

