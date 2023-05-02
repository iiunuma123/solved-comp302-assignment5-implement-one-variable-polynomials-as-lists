Download Link: https://assignmentchef.com/product/solved-comp302-assignment5-implement-one-variable-polynomials-as-lists
<br>
[Question 1:

In this question you will implement one-variable polynomials as lists. Use the following type definitions and exception declaration

type term = Term of float * int type poly = Poly of (float * int) list exception EmptyList

A term like 3<em>.</em>5<em>x</em><sup>8 </sup>is represented as (3<em>.</em>5<em>,</em>8). The exponent in a polynomial is <strong>always </strong>non-negative<a href="#_ftn1" name="_ftnref1"><sup>[1]</sup></a>. The exponent of a term is called its <em>degree</em>. A polynomial is a list of terms arranged so that the first term in the list has the highest degree and thereafter the terms are listed in decreasing order of the degree. We do not write terms that have 0<em>.</em>0 as a coefficient <em>except </em>in the special case where we have the zero polynomial. This is written as a polynomial with only one term Poly [(0.0,0)]. <strong>An empty list is not a valid polynomial</strong>. We also do not allow multiple terms of the same degree. These are just the rules that you <em>normally </em>use when writing polynomials. <strong>Maintaining these restrictions is part of your programming task. </strong>You can <em>assume </em>that the polynomials that you start with are represented correctly, and your outputs must respect these restrictions. You must make sure that you check whether your input is of the form Poly [] which is an illegal input. In this case, raise the exception EmptyList. Apart from this you do not have to check that the input is of the right form. <strong>Please do not include this case in your tests.</strong>

Please implement the following functions:

val multiplyPolyByTerm : term * poly -&gt; poly val addTermToPoly : term * poly -&gt; poly val addPolys : poly * poly -&gt; poly val multPolys : poly * poly -&gt; poly

The function multiplyPolyByTerm multiplies a term and a polynomial. The function addTermToPoly adds a term to a polynomial. These two are then used in the next two functions which add and multiply polynomials respectively.

Code to start you off, including a couple of helpful auxiliary functions will be preloaded in the system..

# let t1 = Term (2.0,2);; val t1 : term = Term (2., 2)

# let p1:poly = Poly [(3.0,5);(2.0,2);(7.0,1);(1.5,0)];; val p1 : poly = Poly [(3., 5); (2., 2); (7., 1); (1.5, 0)] # let p2 : poly = Poly [(3.0, 5); (4.0, 2); (7.0, 1); (1.5, 0)];; val p2 : poly = Poly [(3., 5); (4., 2); (7., 1); (1.5, 0)]

# let p3 = multiplyPolyByTerm(t1,p1);; val p3 : poly = Poly [(6., 7); (4., 4); (14., 3); (3., 2)]

# let p4 = addPolys(p1,p3);; val p4 : poly =

Poly [(6., 7); (3., 5); (4., 4); (14., 3); (5., 2); (7., 1); (1.5, 0)]

# let p5 = multPolys(p1,p3);; val p5 : poly =

Poly

[(18., 12); (24., 9); (84., 8); (18., 7); (8., 6); (56., 5); (110., 4); (42., 3); (4.5, 2)]

[Question 2:

This exercise shows you how to do low-level pointer manipulation in OCaml if you ever need to do that. We can define linked lists as follows:

type cell = { data : int; next : rlist} and rlist = cell option ref

Notice that this is a <em>mutually recursive </em>definition. Each type mentions the other one. The keyword and is used for mutually recursive definitions.

Implement an OCaml function insert which inserts an element into a <em>sorted </em>linked list and <em>preserves the sorting</em>. You do not have to worry about checking if the input list is sorted. The type should be val insert : comp:(int * int -&gt; bool) -&gt; item:int -&gt; list:rlist -&gt; unit

Insert takes in three arguments: A comparison function of type int * int -&gt; bool, an element of type int and a linked list l of type rlist. Your function will <strong>destructively </strong>update the list l. This means that you will have mutable fields that get updated. Please note the types carefully. Here is the code I used to test the program.

let c1 = {data = 1; next = ref None} let c2 = {data = 2; next = ref (Some c1)} let c3 = {data = 3; next = ref (Some c2)} let c5 = {data = 5; next = ref (Some c3)} (* This converts an rlist to an ordinary list. *) let rec displayList (c : rlist) =

match !c with

| None -&gt; []

| Some { data = d; next = l } -&gt; d :: (displayList l)

(* Useful if you are creating some cells by hand and then converting them to rlists as I did above. *) let cell2rlist (c:cell):rlist = ref (Some c)

(* Example comparison function. *) let bigger(x:int, y:int) = (x &gt; y)

You may find the displayList and cell2rlist functions useful. They will be preloaded for you.

Here are examples of the code in action:

# let l5 = cell2rlist c5;; val l5 : rlist = …. (* Messy display deleted. *)

# displayList l5;;

<ul>

 <li>: int list = [5; 3; 2; 1]</li>

</ul>

# displayList l5;;

<ul>

 <li>: int list = [5; 3; 2; 1]</li>

</ul>

# insert bigger 4 l5;;

<ul>

 <li>: unit = ()</li>

</ul>

# displayList l5;;

<ul>

 <li>: int list = [5; 4; 3; 2; 1]</li>

</ul>

# insert bigger 9 l5;;

<ul>

 <li>: unit = ()</li>

</ul>

# displayList l5;;

<ul>

 <li>: int list = [9; 5; 4; 3; 2; 1]</li>

</ul>

# insert bigger 0 l5;;

<ul>

 <li>: unit = ()</li>

</ul>

# displayList l5;;

<ul>

 <li>: int list = [9; 5; 4; 3; 2; 1; 0]</li>

</ul>

The program is short (5 lines or fewer) and <em>easy to mess up</em>. Please think carefully about whether you are creating aliases or not. You can easily write programs that look absolutely correct but which create infinite loops. It might happen that your insert program looks like it is working correctly but then displayList crashes. You might then waste hours trying to “fix” displayList and cursing me for writing incorrect code. Most likely, your insert happily terminated but created a cycle of pointers which then sends displayList into an infinite loop. <strong>We will not ask for test cases for this question.</strong>

[Question 3:

Can you come up with an algorithm that finds the largest and the next largest elements in an unsorted list or array using <em>n </em>+ dlog<sub>2 </sub><em>n</em>e− 2 comparisons? Prove that this is optimal.

<a href="#_ftnref1" name="_ftn1">[1]</a> Polynomials with terms that have negative exponents are called Laurent polynomials and are a completely different kind of mathematical object.