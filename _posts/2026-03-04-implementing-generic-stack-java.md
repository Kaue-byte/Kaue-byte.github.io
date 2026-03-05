---
title: "Post: Implementing a Generic Stack in Java"
categories:
  - Blog
  - Java
tags:
  - data-structures
  - java
---

When I was studying data structures in `Java`, one of the first concepts you encounter is the **Stack**.

Briefly, the `Stack` is a type of data organization, **with the aim of saving data fast**.

In the Stack, you can only put new information on the top. Its not possible view what is below if you do not remove the top, like a real stack.
Basically, you can do some **actions** on a stack, like:
```ruby
push: add new information
pop: return the top and remove the top information
isEmpty: verify if the stack is empty
isFull: verify if the stack is full
peek: look at the top of the stack
size: return the size of the stack
clear: clear all data from the stack
```
Before looking at the code, you need to know what a **Generic** is.
Generics are a `parameter that receives a data type`.
```ruby
public class Stack<T> {
 private T[] element;
}
```
Line 2 creates a generic array

And the last thing you need to understand is this abomination.
```ruby
element = (T[]) new Object[len];
```
In this line, you create an array of Object, and you say to the system: `"hey, treat this array of Object like a T[]"`.

But this sounds **SOOOO** strange.

An example to better illustrate this.
```ruby
Main:
Stack<String> stack = ...
class Stack<String>{
private String[] element;
...
element = (String[]) new Object[len];
}
```
**So what is happening?**

An explicit unchecked cast between incompatible array types caused by type erasure.

So lets try to break this phrase, what is a `cast`?

A cast is a type conversion, so you say to the Java compiler to assume X as Y, but X does not transform into Y, for example:
```ruby
int j =10;
float i = (int) j;
```
What is a `unchecked cast`?
It is a cast that the compiler cannot guarantee is safe at compile time.
```ruby
import java.util.*;
List list = new ArrayList(); // list without type
list.add("Hello");
List<String> strings = list;
```

This is an unchecked cast because Java cannot guarantee the list is `filled with only strings`.

So unchecked cast between incompatible array types is:
```ruby
element = (String[]) new Object[len];
```
Now the `Generics Stacks` implementation:
```ruby
public class Stack<T> {
    private static final int STACK_SIZE = 100;
    private T[] element;
    private int top;
    @SuppressWarnings("unchecked") //Ignore the unchecked warnings
    public Stack(int len){
        element = (T[]) new Object[len];
        top =-1;
    }
    public Stack(){
        this(STACK_SIZE); //Call the Stack() whit the STACK_SIZE as a parameter
    }
    public void push(T value){
        if (isFull()){
            throw new RuntimeException("Stack is full");
        }
        element[++top] = value;
    }
    public T pop(){
        if (isEmpty()) {
            throw new RuntimeException("Stack is empty");
        }
        return element[top--];
    }
    public boolean isEmpty(){
        return top < 0;
    }
    public boolean isFull(){
        return top == element.length -1;
    }
    public T peek(){
        if(top == -1){
            throw new RuntimeException("Stack is empty");
        }
        return element[top];
    }
    public int size(){
        return top + 1;
    }
    public void clear(){
        top = -1;
    }
}
```
