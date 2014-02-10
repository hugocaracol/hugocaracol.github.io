---
layout: post
title: "Bitwise and CONSTANT values"
date: 2013-02-15 22:29
comments: true
categories: [bitwise]
---
Recently I read an article explaining how to use bitwise operations to grant or deny access to something.
I liked the article so much that I'll try to add it to my personal stack (my blog).

There are 4 types of bitwise operators: **NOT**, **AND**, **OR** and **XOR**. 
In Python these operators are **~**, **&**, **|**, **&#94;** respectively.

##Truth tables

####&nbsp;&nbsp;NOT&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;AND&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;OR&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;XOR

<table>
<tr>
<td>
    <table style="border: 1px solid black;">
        <tr>
            <th style="border: 1px solid black;">A</th><th style="border: 1px solid black;text-align:center">NOT A</th>
        </tr>
        <tr>
            <td style="border: 1px solid black;text-align:center">1</td><td style="border: 1px solid black;text-align:center">0</td>
        </tr>
        <tr>
            <td style="border: 1px solid black;text-align:center">0</td><td style="border: 1px solid black;text-align:center">1</td>
        </tr>
    </table>
</td>
<td>&nbsp;&nbsp;&nbsp;&nbsp;</td>
<td>
    <table style="border: 1px solid black;">
        <tr>
            <th style="border: 1px solid black;text-align:center">A</th>
            <th style="border: 1px solid black;text-align:center">B</th>
            <th style="border: 1px solid black;text-align:center">A AND B</th>
        </tr>
        <tr>
            <td style="border: 1px solid black;text-align:center">0</td>
            <td style="border: 1px solid black;text-align:center">0</td>
            <td style="border: 1px solid black;text-align:center">0</td>
        </tr>
        <tr>
            <td style="border: 1px solid black;text-align:center">0</td>
            <td style="border: 1px solid black;text-align:center">1</td>
            <td style="border: 1px solid black;text-align:center">0</td>
        </tr>
        <tr>
            <td style="border: 1px solid black;text-align:center">1</td>
            <td style="border: 1px solid black;text-align:center">0</td>
            <td style="border: 1px solid black;text-align:center">0</td>
        </tr>
        <tr>
            <td style="border: 1px solid black;text-align:center">1</td>
            <td style="border: 1px solid black;text-align:center">1</td>
            <td style="border: 1px solid black;text-align:center">1</td>
        </tr>
    </table>
</td>
<td>&nbsp;&nbsp;&nbsp;&nbsp;</td>
<td>
    <table style="border: 1px solid black;">
        <tr>
            <th style="border: 1px solid black;text-align:center">A</th>
            <th style="border: 1px solid black;text-align:center">B</th>
            <th style="border: 1px solid black;text-align:center">A OR B</th>
        </tr>
        <tr>
            <td style="border: 1px solid black;text-align:center">0</td>
            <td style="border: 1px solid black;text-align:center">0</td>
            <td style="border: 1px solid black;text-align:center">0</td>
        </tr>
        <tr>
            <td style="border: 1px solid black;text-align:center">0</td>
            <td style="border: 1px solid black;text-align:center">1</td>
            <td style="border: 1px solid black;text-align:center">1</td>
        </tr>
        <tr>
            <td style="border: 1px solid black;text-align:center">1</td>
            <td style="border: 1px solid black;text-align:center">0</td>
            <td style="border: 1px solid black;text-align:center">1</td>
        </tr>
        <tr>
            <td style="border: 1px solid black;text-align:center">1</td>
            <td style="border: 1px solid black;text-align:center">1</td>
            <td style="border: 1px solid black;text-align:center">1</td>
        </tr>
    </table>    
</td>
<td>&nbsp;&nbsp;&nbsp;&nbsp;</td>
<td>    
    <table style="border: 1px solid black;">
        <tr>
            <th style="border: 1px solid black;text-align:center">A</th>
            <th style="border: 1px solid black;text-align:center">B</th>
            <th style="border: 1px solid black;text-align:center">A XOR B</th>
        </tr>
        <tr>
            <td style="border: 1px solid black;text-align:center">0</td>
            <td style="border: 1px solid black;text-align:center">0</td>
            <td style="border: 1px solid black;text-align:center">0</td>
        </tr>
        <tr>
            <td style="border: 1px solid black;text-align:center">0</td>
            <td style="border: 1px solid black;text-align:center">1</td>
            <td style="border: 1px solid black;text-align:center">1</td>
        </tr>
        <tr>
            <td style="border: 1px solid black;text-align:center">1</td>
            <td style="border: 1px solid black;text-align:center">0</td>
            <td style="border: 1px solid black;text-align:center">1</td>
        </tr>
        <tr>
            <td style="border: 1px solid black;text-align:center">1</td>
            <td style="border: 1px solid black;text-align:center">1</td>
            <td style="border: 1px solid black;text-align:center">0</td>
        </tr>
    </table>    
</td>
</tr>
</table>

##Left Shift, Right Shift

If we have a byte representing an unsigned integer 1 (0 0 0 0 0 0 0 1) and make a left shift, we get 2 (0 0 0 0 0 0 1 0). If we make another shift to the left 
we get 4 (0 0 0 0 0 1 0 0) and so on.

On the other hand if we have a byte with the unsigned integer 4 (0 0 0 0 0 1 0 0) and shift it to the right, we get 2 (0 0 0 0 0 0 1 0).

In Python **<<** represents left shift and **>>** represents right shift.
<!-- more -->

##Assigning CONSTANT values

Imagine we need to set some permissions to protect some sort of CRUD (Create Read Update Delete) system. Different users have different permissions.
One user may have more than one permission. If he's root (or superman) he may do all 4, but if he's a guest he may only read.

To set all this stuff easily we can start by assigning PERM_CREATE = 1. Next is PERM_READ. But which value this constant will hold? Easy!! 
We can left shift PERM_CREATE and assigning it to PERM_READ. Therefore, PERM_READ = 2. And we do the same thing for the remaining permission items.

In the end we get:  
PERM_CREATE = 1 (**0 0 0 0 0 0 0 1**)  
PERM_READ = 2 (**0 0 0 0 0 0 1 0**)  
PERM_UPDATE = 4 (**0 0 0 0 0 1 0 0**)  
PERM_DELETE = 8 (**0 0 0 0 1 0 0 0**)  

Did you follow the binary pattern represented above? Noticed all the left shifts?

##Making sense of these constants

To give all permissions to the user A we can do:

{% highlight python %}
PERM_A = PERM_CREATE | PERM_READ | PERM_UPDATE | PERM_DELETE
{% endhighlight %}

If we check the value of PERM_A, we get 15.

0 0 0 0 0 0 0 1 **PERM_CREATE**  
0 0 0 0 0 0 1 0 **PERM_READ**  
0 0 0 0 0 1 0 0 **PERM_UPDATE**  
0 0 0 0 1 0 0 0 **PERM_DELETE**  
=-=-=-=-=-=-=-=  
0 0 0 0 1 1 1 1 => 15 (base 10)  

Having PERM_A all permissions, if we want to remove the permission to delete we just '**XOR** it'.  

{% highlight python %}
PERM_A = PERM_A ^ PERM_DELETE
{% endhighlight %}

And finally, to check if the user has permission to delete we just '**AND** it'.

{% highlight python %}
if PERM_A & PERM_DELETE > 0:
    print 'Yes I can!'
{% endhighlight %}

This concludes the bitwise perm system.
