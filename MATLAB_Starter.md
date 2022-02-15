## MATLAB for Starters

This page helps you to learn some basics about MATLAB.

MATLAB is a software for scientific computing. The name stands for _matrix laboratory_. It can handle simple arithmetic calculations like those your scientific calculator can do, as well as complex machine learning algorithms that aids the design of BMW automobiles.


### Arithmetic

Now let's examine MATLAB by doing some simple arithmetic. This help us gain confidence and intuiation in this computing tool we are going to use.

Type `2+6` after the `>>` prompt, followed by **Enter**
```
>> 2+6
```
The commands are executed only after you entering them. Try a few other commands.
```
>> 3-5
>> 2*9
>> 3^2
>> 10/2
>> 1/0
```
Can you figure out what the symbols `*`, `/`, `^`, mean? (They are called **operators**). For the last command, you should get `Inf` (_infinity_). You can also use `Inf` for calculation. Try `12+Inf` and `100/Inf`. Another special value you may meet is `NaN`, which stands for _Not-a-Number_.

### Variables

A variable can be created simply by assigning a value to it at the command line, for example,
```
>> a = 3
```
This statement _**assigns**_ the value of 3 to `a`. Then try the following two lines,
```
>> a = a + 2
>> a = a * 5
```
Do you agree with the final value of `a`? You can create new variables from old ones, like the following,
```
>> x = 5; y =7;
>> z = x + y
```
The first line above does not show any output in the command window, because the semicolon (;) suppress the output of one line being displayed.

### Mathematical Functions

All the common mathematical functions can be found in MATLAB, such as `sin`, `cos`, `log`, `sqrt`. Try the following commands,
```
>> sin(pi)
>> cos(pi/2)
>> cosd(90)
>> sqrt(pi)
>> whos
>> clear
>> whos
```
`whos` lists the current variables in your _**workspace**_. The command `clear` remove variables from memory. You should note that MATLAB knows the value of _π_, because it is one of numerous built-in functions fo MATLAB.

### Vectors

The variables we used above (e.g. `a`, `b`, `x`) are called _**scalars**_, because they are single-valued. If a variable encompasses multiple scalars in one row or one column, it is called a _row vector_ or _column vector_, respectively.
```
>> x = [1, 2, 3, 4, 5]
>> y = [1; 2; 3; 4; 5]
```
A convenient way to create vectors with uniform interval is to use a colon (:). The following lines create row vectors ranging from 1 to 10, though they have different intervals.
```
>> z = [1:10]
```
or
```
>> z = [1:2:10]
```
A column vector can be converted to a row vector by adding a prime symbol (') to its end. This action is called _transpose_. Try the following commands to verify.
```
>> x'
>> size(x')
>> size(y)
>> x' == y
```
The output of the last command is a logical vector.

Like scalars, you can also apply built-in functions to a vector. The following commands can draw a nice graph of sin(x).
```
>> x = [0 : 0.1 : 2*pi];
>> y = sin(x);
>> plot(x, y), grid
```
The graph appears in a separate figure window. You can fine-tune properties of the figure by specifying more options. For example,
```
>> plot(x, y, '-ro', 'LineWidth', 2, 'MarkerEdgeColor', 'b', 'MarkerSize', 5), grid
>> xlabel('x'), ylabel('sin(x)')
>> axis([0 2*pi -1.0 1.0])
```

Previous commands can be selected with Up-arrow and Down-arror on your keyboard. MATLAB has a useful editing feature called _smart recall_. When you type the first few characters of the command you want to recall and then press the Up-arrow key, the command window automatically show previous commands starting with the same first few characters.

It is also useful to know that the command window can do _tab completion_: You type a few letters of a MATLAB name and then press Tab, if the name is unique, it is automatically completed. If it is not, the command window shows you all the possibilities.

### The Desktop and the Editor

Lecturer explain the functions some tabs in the MATLAB IDE, the _current directory_, the _workspace_, the _help_ system ...

Sooner or later, you will meet problems which MATLAB cannot solve in one line. A collection of statements to solve such a problem is called a _program_. Then you will need to generate a script file to save your program.

Create a new script file from the MATLAB desktop. It is generally a good habit to save it first before typing in even your first line of code. MATLAB script is usually saved as a .m file. As an exercise, type the following two lines of code in your script file.
```
>> x = 0 : pi/100 :10*pi;
>> plot(x, exp(-0.1*x) .* sin(x), 'r'), grid
```
After saving the changes. You can run it either by either click the _**run**_ button on the editor screen, or by typing the file name in your command window (you have to change the current directory to where your script file is).

### Exercises

1.1 Assgin values to variables `x` and `y` on the command line, e.g., `x=5` and `y=10`. Write some statements to find the sum, difference, product, and quotient of `x` and `y`.

1.2 For the same `x` and `y` in 1.1, what is the square root of `x`? What is the cosine of the square root of `y`?

1.3 Write a simple program for this problem and save it to a script file. Suppose you have $1000 in the bank. Interest is compounded at the rate of 9% per year. What will your bank balance be after one year? \[_hint: you can use the function `disp` to display your calculation result; type `help disp` on the command line to get more info._] 
