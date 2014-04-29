---
layout: post
title:  Learnning Erlang
date:   2014-04-30 01:46:11
category: "主题"
---

# Learnning Erlang

## Install
- download the source
```
git clone git://github.com/erlang/otp.git
```

- set the ERL_TOP and PATH environment variables:
```
export ERL_TOP=/Users/yejunsun/otp
export PATH=$ERL_TOP/bin:$PATH
```

- The repository does not contain a generated configure file, so it must be generated like this:
```
./otp_build autoconf
```

```
./configure
make
```

##Manual

---------------

```
erl -man lists
```

## what is erlang

------------------------------------------------------

- a general-purpose concurrent, garbage-collected programming language and runtime system. 
- a functional language, with strict evaluation, single assignment, and dynamic typing. 
- For concurrency it follows the Actor model. 
- It was designed by Ericsson to support distributed, fault-tolerant, soft-real-time, non-stop applications.
- It supports hot swapping, thus code can be changed without stopping a system.
- The first case of this is related to Erlang's massive scaling abilities due to its lightweight processes. It is true that Erlang processes are very light: 
- actor model

###Actor model

- The Actor model adopts the philosophy that everything is an actor. This is similar to the everything is an object philosophy used by some object-oriented programming languages, but differs in that object-oriented software is typically executed sequentially, while the Actor model is inherently concurrent.
- An actor is a computational entity that, in response to a message it receives, can concurrently:
- > send a finite number of messages to other actors;
- > create a finite number of new actors;
- > designate the behavior to be used for the next message it receives.

## Code orgnization

-----------------------------

-  Modules are a bunch of functions regrouped in a single file, under a single name
- all functions in Erlang must be defined in modules
-  Attributes are metadata describing the module itself such as its name
- The BIFs mentioned in the previous chapter, like hd or tl, actually belong to the erlang module, as well as all of the arithmetic, logic and Boolean operators. 
- BIFs from the erlang module differ from other functions as they are automatically imported when you use Erlang. 
- Every other function defined in a module you will ever use needs to be called with the form Module:Function(Arguments).

```
1> erlang:element(2, {a,b,c}).
b
2> element(2, {a,b,c}).
b
3> lists:seq(1,4).
[1,2,3,4]
4> seq(1,4).
** exception error: undefined shell command seq/2
```

- When writing a module, you can declare two kinds of things: functions and attributes.

### Attributes
```erlang
-module(useless).
-export([add/2, hello/0, greet_and_add_two/1]).
 
add(A,B) ->
	A + B.
 
%% Shows greetings.
%% io:format/1 is the standard function used to output text.
hello() ->
	io:format("Hello, world!~n").
 
greet_and_add_two(X) ->
	hello(),
	add(X,2).
```

- export
- This is used to define what functions of a module can be called by the outside world. 
```
-export([Function1/Arity, Function2/Arity, ..., FunctionN/Arity]).
```

- import
- importing a function is not much more than a shortcut for programmers when writing their code.

```
-import(Module, [Function1/Arity, ..., FunctionN/Arity]).
```

-  A macro is defined as a module attribute of the form: -define(MACRO, some_value). and is used as ?MACRO inside any function defined in the module. A 'function' macro could be written as -define(sub(X,Y), X-Y). and used like ?sub(23,47), later replaced by 23-47 by the compiler. Some people will use more complex macros, but the basic syntax stays the same.

### Compile

- Erlang code is compiled to bytecode in order to be used by the virtual machine.
- You can call the compiler from many places: $ erlc flags file.erl when in the command line, 
- compile:file(FileName) when in the shell or in a module, 
- c() when in the shell, etc.

```
1> cd("/path/to/where/you/saved/the-module/").
"Path Name to the directory you are in"
ok
2> c(useless).
{ok,useless}
3> useless:add(7,2).
9
4> useless:hello().
Hello, world!
ok
5> useless:greet_and_add_two(-3).
Hello, world!
-1
6> useless:not_a_real_function().
** exception error: undefined function useless:not_a_real_function/0
```

##Structure and Terminology

------------------------------------------

- Erlang systems are divided into modules.
- Modules are composed of functions and attributes.
- Functions are either only visible inside a module or they are exported i.e. they can also be called by other functions in other modules.
- Attributes begin with "-" and are placed in the beginning of a module.

##Data Type

###Number

---------------

- two types of numeric literals, integers and floats.
- two Erlang-specific notations: 
- $char : ASCII value of the character char.
- base#value 
- Integer with the base base

```
1> 42.
42
2> $A.
65
3> $\n.
10
4> 2#101.
5
5> 16#1f.
31
6> 2.3.
2.3
7> 2.3e3.
2.3e3
8> 2.3e-3.
0.0023
```

```
1> 2 + 15.
17
2> 49 * 100.
4900
3> 1892 - 1472.
420
4> 5 / 2.
2.5
5> 5 div 2.
2
6> 5 rem 2.
1
7> (50 * 100) - 4999.
1
8> -(50 * 100 - 4999).
-1
9> -50 * (100 - 4999).
244950
10> 2#101010.
42
11> 8#0677.
447
12> 16#AE.
174

```

### Invariable Variables

-------------------------------

- variables can't be variable in functional programming.
- variables begin with an uppercase letter
- There is a reason why variables names can't begin with a lowercase character: atoms.
```
1> One.
* 1: variable 'One' is unbound
2> One = 1.
1
3> Un = Uno = One = 1.
1
4> Two = One + One.
2
5> Two = 2.        
2
6> Two = Two + 1.
** exception error: no match of right hand side value 3
7> two = 2.
** exception error: no match of right hand side value 2
```
- This behavior of the = operator is the basis of something called 'Pattern matching'

### Atoms

--------------

- An atom is a literal, a constant with name.
- An atom should be enclosed in single quotes (') if it does not begin with a lower-case letter or if it contains other characters than alphanumeric characters, underscore (_), or @.
- Atoms are really nice and a great way to send messages or represent constants. 
- However there are pitfalls to using atoms for too many things: an atom is referred to in an "atom table" which consumes memory (4 bytes/atom in a 32-bit system, 8 bytes/atom in a 64-bit system). 
- The atom table is not garbage collected, and so atoms will accumulate until the system tips over, either from memory usage or because 1048577 atoms were declared.
- This means atoms should not be generated dynamically for whatever reason;

```
hello
phone_number
'Monday'
'phone number'

```

### Boolean Algebra & Comparison operators

-----------------------------------------------------------

```
1> true and false.
false
2> false or true.
true
3> true xor false.
true
4> not false.
true
5> not (true and true).
false
```

- the boolean operators and and or will always evaluate arguments on both sides of the operator. If you want to have the short-circuit operators (which will only evaluate the right-side argument if it needs to), use andalso and orelse.

```
6> 5 =:= 5.
true
7> 1 =:= 0.
false
8> 1 =/= 0.
true
9> 5 =:= 5.0.
false
10> 5 == 5.0.
true
11> 5 /= 5.0.
false
12> 1 < 2.
true
13> 1 < 1.
false
14> 1 >= 1.
true
15> 1 =< 1.
true
```
- Keep an eye on that =<

```
12> 1 < 2.
true
13> 1 < 1.
false
14> 1 >= 1.
true
15> 1 =< 1.
true
12> 5 + llama.
** exception error: bad argument in an arithmetic expression
in operator  +/2
called as 5 + llama
13> 5 =:= true.
false
14> 0 == false.
false
15> 1 < false.
true
```

- Erlang doesn't let you add anything with everything, it will let you compare them.
- Erlang has no such things as boolean true and false. The terms true and false are atoms
- The correct ordering of each element in a comparison is the following:

```
number < atom < reference < fun < port < pid < tuple < list < bit string
```

### Tuples

---------------

- a tuple is written in the form {Element1, Element2, ..., ElementN}.
```
1> X = 10, Y = 4.
4
2> Point = {X,Y}.
{10,4}
3> Point = {4,5}.
{4,5}
4> {X,Y} = Point.
{4,5}
5> X.
4
6> {X,_} = Point.
{4,5}
```

- Note that on expression 6, I used the anonymous _ variable. This is exactly how it's meant to be used: to drop the value that would usually be placed there since we won't use it. The _ variable is always seen as unbound and acts as a wildcard for pattern matching.

```
7> {_,_} = {4,5}.
{4,5}
8> {_,_} = {4,5,6}.
** exception error: no match of right hand side value {4,5,6}
9> Temperature = 23.213.
23.213
10> PreciseTemperature = {celsius, 23.213}.
{celsius,23.213}
11> {kelvin, T} = PreciseTemperature.
** exception error: no match of right hand side value {celsius,23.213}
12> {point, {X,Y}}.
{point,{4,5}}
```

### Lists!

------------

- a list is [Element1, Element2, ..., ElementN]
```
1> [1, 2, 3, {numbers,[4,5,6]}, 5.34, atom].
[1,2,3,{numbers,[4,5,6]},5.34,atom]
```
```
2> [97, 98, 99].
"abc"
```

- Uh oh! This is one of the most disliked things in Erlang: strings! Strings are lists and the notation is absolutely the exact same! Why do people dislike it? Because of this:
```
3> [97,98,99,4,5,6].
[97,98,99,4,5,6]
4> [233].
"é"
```
- 

- Erlang will print lists of numbers as numbers only when at least one of them could not also represent a letter! 
- String is actually a list of integers(ASCII)

- To glue lists together, we use the ++ operator. The opposite of ++ is -- and will remove elements from a list:
```
5> [1,2,3] ++ [4,5].
[1,2,3,4,5]
6> [1,2,3,4,5] -- [1,2,3].
[4,5]
7> [2,4,2] -- [2,4].
[2]
8> [2,4,2] -- [2,4,2].
[]
```

- Both ++ and -- are right-associative. This means the elements of many -- or ++ operations will be done from right to left, as in the following examples:

```
9> [1,2,3] -- [1,2] -- [3].
[3]
10> [1,2,3] -- [1,2] -- [2].
[2,3]
```

- The first element of a list is named the Head, and the rest of the list is named the Tail. We will use two built-in functions (BIF) to get them.

```
11> hd([1,2,3,4]).
1
12> tl([1,2,3,4]).
[2,3,4]
```

- Note: built-in functions (BIFs) are usually functions that could not be implemented in pure Erlang, and as such are defined in C, or whichever language Erlang happens to be implemented on (it was Prolog in the 80's). 

- Accessing or adding the head is fast and efficient
- there is a nicer way to separate the head from the tail of a list with the help of pattern matching: [Head|Tail]

```
13> List = [2,3,4].
[2,3,4]
14> NewList = [1|List].
[1,2,3,4]
15> [Head|Tail] = NewList.
[1,2,3,4]
16> Head.
1
17> Tail.
[2,3,4]
18> [NewHead|NewTail] = Tail.
[2,3,4]
19> NewHead.
2
20> [1 | []].
[1]
21> [2 | [1 | []]].
[2,1]
22> [3 | [2 | [1 | []] ] ].
[3,2,1]
```

### List Comprehensions

---------------------------------

- It's based off the idea of set notation;
- An example of set notation would be {x ∈ ℜ x = x^2}.
```
1> [2*N || N <- [1,2,3,4]].
[2,4,6,8]
2> [X || X <- [1,2,3,4,5,6,7,8,9,10], X rem 2 =:= 0].
[2,4,6,8,10]
```

-  A customer enters, sees our menu and asks if he could have the prices of all the items costing between $3 and $10 with taxes (say 7%) counted in afterwards.

```
3> RestaurantMenu = [{steak, 5.99}, {beer, 3.99}, {poutine, 3.50}, {kitten, 20.99}, {water, 0.00}].
[{steak,5.99},
{beer,3.99},
{poutine,3.5},
{kitten,20.99},
{water,0.0}]
4> [{Item, Price*1.07} || {Item, Price} <- RestaurantMenu, Price >= 3, Price =< 10].
[{steak,6.409300000000001},{beer,4.2693},{poutine,3.745}]
```

- The recipe for list comprehensions in Erlang is therefore NewList = [Expression || Pattern <- List, Condition1, Condition2, ... ConditionN].
- The part Pattern <- List is named a Generator expression. You can have more than one!

```
5> [X+Y || X <- [1,2], Y <- [2,3]].
[3,4,4,5]
```

- Note that the generator expressions coupled with pattern matching also act as a filter:

```
6> Weather = [{toronto, rain}, {montreal, storms}, {london, fog},  
6>            {paris, sun}, {boston, fog}, {vancouver, snow}].
[{toronto,rain},
{montreal,storms},
{london,fog},
{paris,sun},
{boston,fog},
{vancouver,snow}]
7> FoggyPlaces = [X || {X, fog} <- Weather].
[london,boston]
```

### Bit Syntax!

--------------------

- Bit syntax encloses binary data between << and >>, splits it in readable segments, and each segment is separated by a comma.

```
1> Color = 16#F09A29.
15768105
2> Pixel = <<Color:24>>.
<<240,154,41>>
```

- What's more interesting is the ability to pattern match with binaries to unpack content:

```
3> Pixels = <<213,45,132,64,76,32,76,0,0,234,32,15>>.
<<213,45,132,64,76,32,76,0,0,234,32,15>>
4> <<Pix1,Pix2,Pix3,Pix4>> = Pixels.
** exception error: no match of right hand side value <<213,45,132,64,76,32,76,
0,0,234,32,15>>
5> <<Pix1:24, Pix2:24, Pix3:24, Pix4:24>> = Pixels.
<<213,45,132,64,76,32,76,0,0,234,32,15>>
6> <<R:8, G:8, B:8>> = <<Pix1:24>>.
<<213,45,132>>
7> R.
213
8> <<R:8, Rest/binary>> = Pixels.
<<213,45,132,64,76,32,76,0,0,234,32,15>>
9> R.
213
```

- Erlang accepts more than one way to describe a binary segment. Those are all valid:

```
Value
	Value:Size
	Value/TypeSpecifierList
	Value:Size/TypeSpecifierList
```

- Type
- Possible values: integer | float | binary | bytes | bitstring | bits | utf8 | utf16 | utf32
- The TypeSpecifierList is built by separating attributes by a '-'.
- 'bytes' is shorthand for 'binary'.
- 'bits' is shorthand for 'bitstring'. 
- When no type is specified, Erlang assumes an 'integer' type.

- Signedness
- Possible values: signed | unsigned
- Only matters for matching when the type is integer. The default is 'unsigned'.

- Endianness
- http://en.wikipedia.org/wiki/Endianness
- Possible values: big | little | native
- Endianness only matters when the Type is either integer, utf16, utf32, or float. 
- By default, endianness is set to 'big'.

- Unit
- written unit:Integer
- This is the size of each segment, in bits. The allowed range is 1..256 and is set by default to 1 for integers, floats and bit strings and to 8 for binary. The utf8, utf16 and utf32 types require no unit to be defined. The multiplication of Size by Unit is equal to the number of bits the segment will take and must be evenly divisible by 8. The unit size is usually used to ensure byte-alignment.

```
10> <<X1/unsigned>> =  <<-44>>.
<<"Ô">>
11> X1.
212
12> <<X2/signed>> =  <<-44>>. 
<<"Ô">>
13> X2.
-44
14> <<X2/integer-signed-little>> =  <<-44>>.
<<"Ô">>
15> X2.
-44
16> <<N:8/unit:1>> = <<72>>.
<<"H">>
17> N.
72
18> <<N/integer>> = <<72>>.
<<"H">>
19> <<Y:4/little-unit:8>> = <<72,0,0,0>>.     
<<72,0,0,0>>
20> Y.
72

```

- The standard binary operations (shifting bits to left and right, binary 'and', 'or', 'xor', or 'not') also exist in Erlang. Just use the functions bsl (Bit Shift Left), bsr (Bit Shift Right), band, bor, bxor, and bnot.

```
2#00100 = 2#00010 bsl 1.
2#00001 = 2#00010 bsr 1.
2#10101 = 2#10001 bor 2#00101.

```

- With that kind of notation and the bit syntax in general, parsing and pattern matching binary data is a piece of cake. One could parse TCP segments with code like this:

```
<<SourcePort:16, DestinationPort:16,
AckNumber:32,
DataOffset:4, _Reserved:4, Flags:8, WindowSize:16,
CheckSum: 16, UrgentPointer:16,
Payload/binary>> = SomeBinary.
```

- There's a whole other aspect to binary notation: bit strings. Binary strings are bolted on top of the language the same way they are with lists, but they're much more efficient in terms of space.

```
21> <<98>>.
<<"b">>
22> <<A:8,B:8,C:8,D:8>> = <<"this">>.
<<"this">>
23> A.
116
24> B.
104
25> C.
105
26> D.
115
```

### Binary Comprehensions

- Binary comprehensions are to bit syntax what list comprehensions are to lists
```
1> [ X || <<X>> <= <<1,2,3,4,5>>, X rem 2 == 0].    
[2,4]
```

- The only change in syntax from regular list comprehensions is the   <- which became <= and using binaries (<<>>) instead of lists ([]). 

```
2> Pixels = <<213,45,132,64,76,32,76,0,0,234,32,15>>.
<<213,45,132,64,76,32,76,0,0,234,32,15>>
3> RGB = [ {R,G,B} || <<R:8,G:8,B:8>> <= Pixels ].
[{213,45,132},{64,76,32},{76,0,0},{234,32,15}]
4> << <<R:8, G:8, B:8>> ||  {R,G,B} <- RGB >>.
<<213,45,132,64,76,32,76,0,0,234,32,15>>
5> << <<Bin>> || Bin <- [<<3,7,5,4,7>>] >>.
** exception error: bad argument
6> << <<Bin/binary>> || Bin <- [<<3,7,5,4,7>>] >>. 
<<3,7,5,4,7>>
7> << <<(X+1)/integer>> || <<X>> <= <<3,7,5,4,7>> >>.
<<4,8,6,5,8>>
```






