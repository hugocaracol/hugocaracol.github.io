---
layout: post
title: "Stack data structure"
tags:
- python
- stack
categories: [data structures]
status: publish
published: true
---
Imagine you have to do the dishes and you put one plate on top of the other.  
The stack data structure works in that way. In the beginning the stack is empty (no plates). The first plate is put onto the stack and now the stack has one plate. There comes another plate which is put on top of the first one. Now you have two plates but you need to put them in the kitchen cabinet. You remove the last plate (the second one) from the stack and put it in the cabinet. You do the same for the other plate (the first one). This is how things work when we talk about the stack data structure.
<!-- more -->
This data structure permits only two operations:  
- **Push** - pushes elements onto the stack  
- **Pop** - removes elements from the stack

This is a "Last In First Out" memory, because the first stored elements are the last to be removed.

I'll show you my implementation (Python) using linked lists.

{% highlight python %}
class Stack:
    def __init__(self):
        self.top = None

    def push(self, obj):
        node = StackNode(obj)
        if self.top is not None:
            node.next = self.top
            self.top = node
        else:
            self.top = node

    def pop(self):
        if self.top is None:
            return None
        node = self.top
        self.top = self.top.next

        return node.item


class StackNode:
    def __init__(self, item):
        self.item = item
        self.next = None
{% endhighlight %}

When the stack object is created, the top property is set to None because the stack is empty.

When one object is pushed onto the stack, we create a new node and check if the stack is empty. If it is, we initialize the stack with that node (self.top = node). If it's not, we set the next property of the node to the top-most node already on the stack and update the top of the stack to the new node.

When we pop a node from the stack we have to check if the stack is empty. If it is, we return nothing. If it's not, we put the self.top node into a variable node, update the top of the stack to the next node, and return the object that is contained in the node.

Usage example:

{% highlight python %}
stack = Stack()  # creates the stack object
stack.push('Plate1')
stack.push('Plate')
#then we pop them
print stack.pop()  # Plate2
print stack.pop()  # Plate1
{% endhighlight %}

I hope it's clear.
