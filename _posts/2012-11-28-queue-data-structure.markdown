---
layout: post
title: "Queue data structure"
tags:
- python
- queue
categories: [data structures]
status: publish
published: true
---
You're at a reception desk. There are four people waiting in front of you and you have to wait for your turn. That's how queues work.
The queue is a data structure where the first elements to arrive are the first to be served. First come, first served or "First In First Out".


This data structure permits two operations:  
- **Enqueue** - inserts an element into a queue  
- **Dequeue** - removes an element from a queue

This is my implementation (Python) with linked lists:

<!-- more -->

{% highlight python %}
class Queue:
    def __init__(self):
        self.head = None
        self.tail = None

    def Enqueue(self, obj):
        node = QueueNode(obj)
        if self.tail is None:
            self.head = node
        else:
            self.tail.next = node
        self.tail = node

    def Dequeue(self):
        if self.head is None:
            return None
        else:
            node = self.head
            if self.head.next is None:
                self.head = None
                self.tail = None
            else:
                self.head = self.head.next

        return node.item


class QueueNode:
    def __init__(self, item):
        self.item = item
        self.next = None
{% endhighlight %}

When the queue object is created, the head and tail properties are set to None. The 'head' references the first item to be processed and the 'tail' references the last node in the queue after wich we add the new node.

When an object is enqueued (added to the queue) we create a node object. Then we have to check if the queue is empty. If it is, the 'head' and 'tail' both point to that node. If it's not, the 'next' property of the last node (referenced by 'tail') is set to the new node.

When we want to dequeue an object, we have to check if the queue is empty. If it is, the method returns nothing. If it's not, we get the node and the 'head' now points to the next node in the queue (self.head.next). If there are no more nodes in the queue, both 'head' and 'tail' are set to None. In the end, the object is returned by 'return node.item'.

This is the basic functionality of the queue data structure.

Usage:

{% highlight python %}
queue = Queue()  # creates the queue object
queue.Enqueue(10)  # enqueue
queue.Enqueue(99)  # enqueue
print queue.Dequeue()  # 10
print queue.Dequeue()  # 99
{% endhighlight %}
