Programming Language Design Assignment 01
===

###### tags: `Programming Language Design 2020` `Assignment` `NCU`

[HackMD Link](https://hackmd.io/@C5qogZpXS6m0aedcVROJ6A/rkPbHEsHw)

> Please hand-in your answers on new ee-class before 10/1 00:00 and
> make sure you’re following these rules or you will get penalty:

> - Submit your answers in a single pdf file. (only pdf files will be accepted)
> - Name your pdf file by your student ID#. (ex. 108522009.pdf)
> - Use monospace-style font to format all the program codes you wrote and DO NOT use code screenshots as your answer.

## Dynamic and Lexical Binding (30pt)

### Question

a.
~~~lisp
(let ((adder (lambda (x) (lambda (y) (+ x y)))))
    (let ((add10 (funcall adder 10)))
        (let ((x 30))
            (funcall add10 x))))
~~~

b.
~~~lisp
(let ((x 1) (y 2) (z 3))
  (let ((y 10) (z 20) (foo (lambda (y) (+ x y z))))
    (let ((x 100) (y 200) (z 300))
      (funcall foo x))))
~~~

> * lexical -> `(setq-local lexical-binding t)`
> * dynamic -> `(setq-local lexical-binding nil)`
> * [Emacs Lisp Basics](http://ergoemacs.org/emacs/elisp_basics.html)
> * [Try It Online](https://tio.run)

### Answer

1. Explain what are **lexical-binding and dynamic-binding** ?
    - lexical binding
        - The binding that happened during compilation.
    - dynamic binding
        - The binding that happened when the program is running.
2. Show the result of above code evaluated in both **lexical-binding and dynamic-binding** lisp interpreters. 

    - lexical binding
    a. 40
    b. 104 
    - dynamic binding
    a. 60
    b. 500


## Breaking the Loop (15pt)

### Question
Java provides a feature called [Labeled Break] which allows programmer to specify which loop to break.
Rewrite the following java code with labeled break to remove unnecessary `break`.

~~~java
public boolean search(List<List<Integer>> nestedList, Integer num) {
    boolean found = false;
    for (List<Integer> list : nestedList) {
        if (found)
            break;
        for (Integer x : list) {
            if (x == num) {
                found = true;
                break;
            }
        }
    }
    return found;
}
~~~

### Answer

~~~java
public boolean search(List<List<Integer>> nestedList, Integer num) {
    boolean found = false;
    
    cool_label:
    for (List<Integer> list : nestedList) {
        for (Integer x : list) {
            if (x == num) {
                found = true;
                break cool_label;
            }
        }
    }
    return found;
}
~~~

## Eager vs Lazy (20pt)

### Question
~~~python
def infinity():
    return infinity()
    
def first(x, y):
    return x
    
print(first(100, infinity()))
~~~

Explain what will happens when the **pseudo code** above being executed by both **eager and lazy** evaluation?

### Answer
- **eager evaluation**: will cause infinite loop
- **lazy evaluation**: print 100 as output

## Parameter-Passing Variation (20pt)
### Question

~~~psudo code
global x,y;
procedure Main:　　　　　　　　   
    begin              　　　　　　　　　
        x=5;  　　　　　　　　　　    
        y=2;　　　　　　　　　　　  
        a(x,x-y);　　　　　　　   
        print(x);　　　　　　　　　   
      end;　　　　　　　　　　　　　  
　　　　　　　　　　　　　　　　　　  　　　　　　　　　　　　　　　　　　  
procedure a(i,j):　　　　　　　  
    begin　　　　　　　　　    　　　
        i=i+1;　　　　　　　　　　　
        print(x);　　　　　　
　　 　　i=i+j;　　　　　　　　　　　  
　　 　　print(i);　　　　　　　　　　  
　　 end;　　　
~~~
~~~
Output:
a. 6,10,10
b. 5,9,5
c. 6,9,9
~~~
a. b. c. are three kinds of outputs using **different parameter-passing method**.
What kind of method do each output use?

> There are three methods for this question:
> 1. call-by-value
> 2. call-by-reference
> 3. call-by-name

### Answer
- a. call-by-name
- b. call-by-value
- c. call-by-reference

## Error Handling (15pt)
### Question
The following code shows a common pattern using `goto` to handling errors in C lang.  
There is a modern common language construct exists and famous for error handling.

> You may rewrite the code in pesudo code or any language provides this feature.

~~~c
int foo(int bar)
{
    int ret_val = 0;

    r1 = allocate_resources_1();

    if (!r1 || !do_something(r1, bar))
        goto error_1;

    r2 = allocate_resources_2();

    if (!r2 || !init_stuff(r2, bar))
        goto error_2;

    r3 = allocate_resources_3();

    if (!r3 || !prepare_stuff(r3, bar))
        goto error_3;

    ret_val = do_the_thing(bar);

error_3:
    cleanup_3(r3);
error_2:
    cleanup_2(r2);
error_1:
    cleanup_1(r1);
    return ret_val;
}
~~~


### Answer
1. What is it? 
- Exception handling

2. Rewrite the code with this construct.

    ```python
    def foo(bar):
        ret_val = 0
        try:
            r1 = allocate_resources_1()
            do_something(r1, bar)
        except:
            cleanup_1(r1)
            return ret_val
        cleanup_1(r1)

        try:
            r2 = allocate_resources_2()
            do_something(r2, bar)
        except:
            cleanup_2(r2)
            return ret_val
        cleanup_2(r2)

        try:
            r3 = allocate_resources_3()
            do_something(r3, bar)
        except:
            cleanup_3(r3)
            return ret_val
        cleanup_3(r3)

        return ret_val
    ```
