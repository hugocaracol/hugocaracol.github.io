---
layout: post
title: "Hash Table"
tags:
- hash table
- python
status: publish
published: true
categories: [data structures]
---
A hash table is a data structure that can map keys to values (objects). If we want to search for a value associated with a key in a table with millions of records, this data structure can be put in a good use.
<!-- more -->
##How it works

The hash table has a defined number of places where keys and values can be stored. These places are called buckets. Assuming our table has 10 buckets, that means we have 10 possible places where we can store our data. The number of buckets can be increased as the table gets a low number of available buckets, but this situation is out of the scope of this post.

When we want to store a key/value pair, the hash table needs to find a place. In order to do that it uses a hash function that transforms the key into a number. With that number it computes an index on the table as number_returned_by_hash_function  % number_of_buckets. For example, for a number of buckets of 10 and a number returned by the hash function of 5604, the index of the table is 4.

After the index is determined, the algorithm tries to store the value. What about if the table on that index already has some value? If that happens we call it a "collision". There are some techniques that can be used to fix that.


##Collision resolution
There are two popular collision resolution techniques: Separate Chaining and Open Addressing.
###Separate Chaining
Each bucket is a pointer to a linked list of key/value pairs. In this way each pair is put in the end of the list, creating a linked list.
###Open Addressing
Each bucket has only a key/value pair. If the bucket is already filled, the algorithm must find a new place to store the key/value. For this discovery Linear Probing and Quadratic Probing can be used.
####Linear Probing
As the bucket is aready used, the next bucket will be the next possibility. If this is also already used, it tries the next one, and so on, until there are no buckets available.

{% highlight python %}
pos = (offset + 1) % self.n_buckets
{% endhighlight %}
####Quadratic Probing
As the bucket is already used, the interval between buckets is increased in a quadratic way until there are no buckets available.

{% highlight python %}
pos = (offset + n_try * n_try) % self.n_buckets
{% endhighlight %}

These collision resolution techniques are used for setting and getting.

Below is my implementation (Python) in a way I could understand.

{% highlight python %}
class Hash:
    '''
    A class for experimenting how to create hash tables.

    MyHash uses djb2 hash function for offset calculation
    and separate chaining for collision resolution.
    '''
    def __init__(self, n_buckets):
        self.n_buckets = n_buckets
        self.hashTable = [None] * n_buckets
        self.fn_collisions = self.collisions_separate_chaining
        # open addressing
        # self.fn_collisions = self.collisions_linear_probing
        # self.fn_collisions = self.collisions_quadratic_probing

    def _hashthis(self, key):
        hash_djb2 = 5381
        for i in range(len(key)):
            hash_djb2 = ((hash_djb2 << 5) + hash_djb2) + ord(key[i])
        return hash_djb2 % self.n_buckets

    def set(self, key, value):
        offset = self._hashthis(key)
        return self.fn_collisions('SET', offset, key, value)

    def get(self, key):
        offset = self._hashthis(key)
        if self.hashTable[offset] is None:
            return None
        else:
            return self.fn_collisions('GET', offset, key)

    def collisions_separate_chaining(self, action, offset, key, value=None):
        if action == 'SET':
            if self.hashTable[offset] is None:
                self.hashTable[offset] = [(key, value)]
            else:
                for i in range(len(self.hashTable[offset])):
                    if self.hashTable[offset][i][0] == key:
                        # updates the value of existing key
                        self.hashTable[offset][i] = (key, value)
                        return True
                self.hashTable[offset].append((key, value))
            return True
        elif action == 'GET':
            for item in self.hashTable[offset]:
                if item[0] == key:
                    return item[1]
            return None
        return False

    def collisions_linear_probing(self, action, offset, key, value=None):
        if action == 'SET':
            if self.hashTable[offset] is None:
                self.hashTable[offset] = (key, value)
                return True
            else:
                pos = (offset + 1) % self.n_buckets
                while pos != offset:
                    if self.hashTable[pos] is None:
                        self.hashTable[pos] = (key, value)
                        return True
                    else:
                        pos = (pos + 1) % self.n_buckets
                # couldn't find any available buckets
                return False
        elif action == 'GET':
            pos = (offset + 1) % self.n_buckets
            while pos != offset:
                if self.hashTable[pos][0] == key:
                    return self.hashTable[pos][1]
                else:
                    pos = (pos + 1) % self.n_buckets
            return None
        return False

    def collisions_quadratic_probing(self, action, offset, key, value=None):
        if action == 'SET':
            if self.hashTable[offset] is None:
                self.hashTable[offset] = (key, value)
                return True
            else:
                n_try = 1
                pos = (offset + n_try * n_try) % self.n_buckets
                while pos != offset:
                    if self.hashTable[pos] is None:
                        self.hashTable[pos] = (key, value)
                        return True
                    else:
                        n_try += 1
                        pos = (pos + n_try * n_try) % self.n_buckets
                # couldn't find any available buckets
                return False
        elif action == 'GET':
            n_try = 1
            pos = (offset + n_try * n_try) % self.n_buckets

            while pos != offset:
                if self.hashTable[pos][0] == key:
                    return self.hashTable[pos][1]
                else:
                    n_try += 1
                    pos = (pos + n_try * n_try) % self.n_buckets
            return None
{% endhighlight %}

In the creation of the object Hash, separate chaining is used for collision resolution. If you want to try the other two, please uncomment the line you want.

{% highlight python %}
self.fn_collisions = self.collisions_separate_chaining
# open addressing
# self.fn_collisions = self.collisions_linear_probing
# self.fn_collisions = self.collisions_quadratic_probing
{% endhighlight %}

Usage:

{% highlight python %}
ht = Hash(10)  # 10 buckets
ht.set('name', 'Hugo')
ht.set('age', 100)
print ht.get('name')  # Hugo
print ht.get('age')  # 100
{% endhighlight %}
