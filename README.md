Download Link: https://assignmentchef.com/product/solved-csci1913-lab-13
<br>
This laboratory assignment involves designing a perfect hash function for a small set of strings. It demonstrates that a perfect hash function need not be hard to design, or hard to understand.

<ol>

 <li></li>

</ol>

We’ll start by reviewing some terminology from the lectures. A <em>hash function</em> is a function that takes a <em>key</em> as its argument, and returns an index into an array. The array is called a <em>hash table.</em> The object that appears at the index in that array is the key’s <em>value.</em> The key’s value is somehow associated with the key.

A hash function may return the same index for two different keys. This is called a <em>collision.</em> Collisions are undesirable if we want distinct values to be associated with distinct keys. A hash function that has no collisions for a set of keys is said to be <em>perfect</em> for that set. Note that a hash function may be perfect for some sets of keys, but not perfect for others.

Most modern programming languages use a small set of <em>reserved names</em> as operators, punctuation, and syntactic markers. (They’re also called <em>reserved words</em> or <em>keywords.</em>) For example, Java currently uses reserved names like if, private, while, etc.

A compiler for a programming language must be able to test if a name in a program is reserved. Programs may be hundreds or thousands of pages long, and may contain thousands or even millions of names. As a result, the test must be done efficiently. It might be implemented using a hash table and a perfect hash function.

Here’s how the test may work. Suppose that the hash table <em>T</em> is an array of strings. Each time the compiler reads a name <em>N</em>, it calls a perfect hash function <em>h</em> to compute an index <em>h</em>(<em>N</em>). If <em>h</em>(<em>N</em>) is a legal index for <em>T,</em> and <em>T</em>[<em>h</em>(<em>N</em>)] = <em>N</em>, then the name is reserved, otherwise it is not. Unused elements of <em>T</em> might be empty strings “”. If we measure the efficiency of a test by the number of string comparisons it performs, then the test requires only <em>O</em>(1) comparisons. Of course this works only if <em>h</em> is perfect for the set of reserved names.       Now suppose there is a very simple programming language that uses the following set of twelve reserved names.

and else    or begin end return define  if then

do     not    while

We might define a perfect hash function for the reserved names in the following way. We get one or more characters from each name. Then we convert each character to an integer. This is easy, because characters are already represented as small nonnegative integers. For example, in the ASCII and Unicode character sets, the characters ‘a’ through ‘z’ are represented as the integers 97 through 122, without gaps. Finally, we do some arithmetic on the integers to obtain an index into the hash table. We choose the characters, and the arithmetic operations, so that no two reserved names result in the same index.

For example, if we define the hash function <em>h</em> so that it adds the first and second characters of each name, we get the following indexes.

<em>h</em>(“and”)        =  207 <em>h</em>(“begin”)  =  199 <em>h</em>(“define”)  =  201

<table width="102">

 <tbody>

  <tr>

   <td width="54"><em>h</em>(“do”)</td>

   <td width="49">  =  211</td>

  </tr>

  <tr>

   <td width="54"><em>h</em>(“else”)</td>

   <td width="49">  =  209</td>

  </tr>

  <tr>

   <td width="54"><em>h</em>(“end”)</td>

   <td width="49">  =  211</td>

  </tr>

  <tr>

   <td width="54"><em>h</em>(“if”)</td>

   <td width="49">  =  207</td>

  </tr>

  <tr>

   <td width="54"><em>h</em>(“not”)</td>

   <td width="49">  =  221</td>

  </tr>

  <tr>

   <td width="54"><em>h</em>(“or”)</td>

   <td width="49">  =  225</td>

  </tr>

 </tbody>

</table>

<em>h</em>(“return”)  =  215 <em>h</em>(“then”)       =  220 <em>h</em>(“while”)  =  223

This definition for <em>h</em> does not result in a perfect hash function, because it has collisions. For example, the strings “and” and “if” result in the index 207. Similarly, the strings “do” and “end” result in the index 211. We either didn’t choose the right characters from each string, or the right operations to perform on those characters, or both. Unfortunately, there is no good theory about how to define <em>h</em>. The best we can do is try various definitions, by trial and error, until we find one that is perfect.

<ol start="2">

 <li><strong> Implementation.</strong></li>

</ol>

Design a perfect hash function for the reserved names shown in the previous section. To do that, write a small test class, something like this, and run it with various definitions for the function hash. It calls hash for each reserved name, and writes indexes to standard output.

class Test

{    private static final String [] reserved =     { “and”,

“begin”,

“define”,

“do”,

“else”,

“end”,

“if”,

“not”,

“or”,

“return”,

“then”,

“while” };




private static int hash(String name)

{

//  Your code goes here.

}




public static void main(String [] args)

{

for (int index = 0; index &lt; reserved.length ; index += 1)

{

System.out.print(“h(”” + reserved[index] + “”) = “);

System.out.print(hash(reserved[index]));

System.out.println();

}    }

}

When defining hash, you might try adding characters at specific indexes from each name. You might try linear combinations of the characters: that is, multiplying the characters by small constants, then adding or subtracting the results. You might try the operator %. You might also try a mixture of these. Whatever you try, reject any definition of hash that is not perfect: one that returns the same index for two different names.

Your method hash must work in constant time, without loops or recursions. It must not use if’s or switch’es. It must not call the Java method hashCode, because that uses a loop, and so does not work in <em>O</em>(1) time. It must not return negative integers, because they can’t be array indexes.

The character at index <em>k</em> in name is obtained by name.charAt(<em>k</em>). Characters in Java Strings are indexed starting from 0, and ending with the length of the string minus 1. For example, the first character from name is returned by name.charAt(0), the second character by name.charAt(1), and the last character by name.charAt(name.length() – 1).

Don’t worry if there are gaps between the indexes: your hash function need not be <em>minimal.</em> Also, try to keep the returned indexes small: they shouldn’t exceed 2000. For example, I know a perfect hash function for the reserved words in this assignment, whose indexes range from 1177 to 1413. I found it after about ten minutes of trial-and-error search.