# <h1 align="center">Regexes -> Text Processing</h1>

<p>Regular expressions (called REs, or regexes, or regex patterns) are essentially a tiny, highly specialized programming language embedded inside Python and made available through the re module.</p>
<p>
* Regex is essentially a search query for a text. i.e when you run a search against a particular piece of text, anything that matches a regular expression is returned as a result of the search.
</p>
<p>* Regexes lets you answer questions like:
  <ul>
    <li> What are all the four letter words in a file?</li>
    <li> How many diffrent error types are there in the given error log?</li>
    .
    .
    .
    .
    .
    </ul>
  </p>
  <hr>
  ==> We can use regular expressions in a whole wide range of programming languages or we can use some command line tools like grep, sed or auk or we can also 
      use regex inside text processing tools or document editors.
  <hr>
  
  <h2> WHY USE REGEX? </h2> THE ANS LIES IN THE <b><u> POWER and FLEXIBILITY</b></u> of regexes. 
  <p> eg. let's say we have a log entry<br>
          log="July 31 07:51:48 mycomputer badprocess[12345]: ERROR Performing package upgrade."
          
          -> let's say you want to extract the process identifier.
  
| One way we can do this is:                                                                                                                                                                                                                                                                                                                      | using the search function of re module:                                                                                                                |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------|
| index=log.index("[") <br>print(log[index+1:index+6]) <br>        <br>o/p: 12345<br>        <br>        Problems associated:<br>        => We not sure that how much long the PID<br>           string would be in all cases.<br>        => What if the line contains another square bracket <br>           before PID? <br><br><br><br><br><br> | import re<br>regex=r"\[(\d+)\]"<br>result=re.search(regex,log)<br>print(result[1])<br>          <br>o/p: 12345<br><br><br><br><br><br><br><br><br><br> |
  

<hr> LET'S GET STARTED!
<h2> Basic Regular Expressions</h2>

* Use of re module<br>
 =>import re<br>
     result=re.search(r"aza","plaza")<br>
     print(result)

     o/p: <re.Math object; span=(2,5), match='aza'>

     Explanation: r indicates that it's a raw string.
                  Always use raw strings for regular expressions in python.
                  Here, pattern="aza"
                        string="plaza" #string in which we want to search our pattern out!
                  In the output, we get the match object [ it means that we got a match in the string ]
                  span attribute=> indicates the range in which substring is found.  


   <b>What if match not found??</b><br>
   result=re.search(r"a2a","plaza")<br>
   print(result)

   o/p: None
   <hr>

<h2> Some Reserved characters: Reserved characters allows us to do more advanced matching. </h2>
=> A <b>dot (.)</b> matches any character<br>    
eg. print(re.search(r"p.ng","penguin"))<br>       

    o/p: <re.Match object; span=(0,4), match='peng'> 
 * We can use this concept to look for entries in a log file that match a certain format.
                                   or
 * To find rows in a csv file that share same characteristics even if they are not exactly the same.
 <br>


 => A <b>circumflex or caret (^)</b> indicates the beginning of a text line.<br>
 eg. to check if a string starts with a particular char,<br>
     print(re.search(r"^x","xenon"))

     o/p: <re.Match object; span=(0,1), match='x'>  #indicating match found! 

  <br>


 => A <b>dollar ($)</b> indicates the end of a text line.<br>
 eg. to check if a string ends with a particular char,<br>
     print(re.search(r"n$","xenon"))

     o/p: <re.Match object; span=(4,5), match='n'>  #indicating match found! 
<hr>

<b>Excercise: </b>Check if the text passed contains the vowels a,e and i with exactly one occurence of any other character in between.
<br>
<I> Link: [Problem 1] (https://github.com/pdesai878/Regexes/blob/8196cba8b49779fc4f25e2ed7510692087c2ff24/Exercises/1.ipynb)</I>
<hr>

<h2> Wildcards and Character Classes</h2>
-> dot '.' is a special character that can match any character. Right?
<p> In <b> Regex World</b>, this is known as a <b>wildcard</b> because it can match more than one character.</p>
<I> Getting more stricter...............</I>
<p> We use another feature of regexes called <b>Character Classes</b>
        <p>-> written inside square brackets.<br>
           -> it lets us list the characters we want to match inside[]</p></p>

   eg. say we want to match the word Python but allow for both lowercase or uppercase p
       -> print(re.search(r"[Pp]ython","Python"))
       o/p: <re.Match object; span=(0,6), match='Python'>

   * Inside the square brackets, we can also define a range of characters using a dash
   eg. if we wanted to look for a string "way" preceeded by any letter, then we write:
       ->print(re.search(r"[a-z]way","The end of the highway"))
       o/p: <re.Match object; span=(18,22), match="hway">

       ->print(re.search(r"[a-z]way","What a way to go!"))
       o/p: None

    * Similarly, we can use [A-Z]
                            [0-9]
                            .
                            .
                            .
      We can combine as many ranges and symbols as we want:
      eg. print(re.search(r"cloud[a-zA-Z0-9]","cloudy"))
          o/p: <re.Match object; span=(0,6), match="cloudy">
<hr>
<b>Exercise: </b>Check if the text passed contains . , : ; ? ! 
<br>
<I> Link: [Problem 2] (https://github.com/pdesai878/Regexes/blob/8196cba8b49779fc4f25e2ed7510692087c2ff24/Exercises/2.ipynb) </I>
<hr>


* Sometimes you may also want to match any characters that aren't in a group. To do that, we use a circumflex inside square brackets [^]. 
  <pre>
  eg. let's create a search pattern which looks for any character that is not a letter
      print(re.search("r[^a-zA-Z]","This is a sentence with spaces.")) 
      o/p: re.Match object; span=(4,5), match=' '
                        |
                        |
                        |
                       \_/  
  eg. What if we also add a space to the list of characters that we dont want to match?
      print(re.search("r[^a-zA-Z ]","This is a sentence with spaces."))     #we added a space to the character class.      
      o/p: re.Match object; span=(4,5), match='.'
      </pre>
   
* If we want to match either one expression or the other, we can use <b> pipe | symbol</b>
  <pre>
  eg. we can have an exp that matches either the word cat or the word dog.
      print(re.search(r"cat|dog","I like cats"))
      o/p: re.Match object; span=(7,10), match='cat'
                        |
                        |
                        |
                       \_/  
  eg. What if we have two possible matches?
      print(re.search(r"cat|dog","I like dogs and cats"))   
      o/p: re.Match object; span=(7,10), match='dog'       #first match is our result

</pre>
* <b>findall()</b> function=> provided by the re module. If we want to get all possible matches, we can do that using findall().
  <pre>
  eg. print(re.findall(r"cat|dog","I like dogs and cats"))
  o/p ['dog','cat']
  </pre>
  <hr>
  
<h2> Repitition Qualifiers </h2>
==> So far we'hv looked at how to match one specific character or a group of characters. Now we are going to check, how to match these characters several times.
    Well, it will become more clear through examples.<br><br>
    - <b> Repeated Matches concept: </b> expression having dot followed by a star. i.e &nbsp &nbsp<b>.*</b>&nbsp &nbsp   in an expression matches any character repeated as many times as possible including 0 times.
    <pre>
    print(re.search(r"Py.*n","Pygmalion")
    o/p: re.Match object; span=(0,9), match='Pygmalion'
                        |
                        |
                        |
                       \_/ 
    #The above expression can be interpreted as match Py followed by any number of characters followed by n.
    </pre>
    <pre> star * matches as many characters as possible. i.e the match length will always be longest. It is known as <b>Repition Qualifier</b>
          eg.  print(re.search(r"Py.*n","Python Programming")
               o/p re.Match object; span=(0,17), match='Python Programmin'
      HOLD ON!!
      |
      | _ _\ we can perform the same operation using character class too.
           / eg. print(re.search(r"Py[a-z]*n","Python Programming")
                re.Match object; span=(0,6), match='Python' </pre>

            # so, if we use the character class, we can get the shortest match.
           
<br>            
<b> -There are two additional repitition qualifiers as well  +  and  ? </b><br><br>
     ==> The plus + rq(repitition qualifier) matches one or more occurence of the char that comes after it.
         The difference between * and + is that + requires atleast one occurence. For * , 0 or more occurences works as well.
         
   <pre>
   eg. print(re.search(r"+o", "wooly"))
       o/p: re.Match object; span=(1,3), match='oo'
       
   eg. print(re.search(r"o+l","woolly"))
       o/p: re.Match object; span=(1, 4), match='ool'
       
   eg. print(re.search(r"w+","woolly"))
       o/p: re.Match object; span=(0, 1), match='w'
       
   eg. <b>Highlighting diff between * and +</b>
       print(re.search(r"z+","woolly"))
       o/p: None
      
            v/s
            
       print(re.search(r"z*","woolly"))
       o/p: re.Match object; span=(0, 0), match=''
       
   eg. print(re.search(r"o+l+","woolly"))
       o/p: re.Match object; span=(1, 5), match='ooll'
       
   eg. print(re.search(r"o+l+","coil"))
       o/p: None                        #the string passed here had another character between o and l  i.e  i.
                                         becaz of which, it did not match with the search pattern.
                                         
   </pre>
   
<hr>
<b>Excercise: </b>Check if the text passed contains letter 'a' (lowercase or uppercase) atleast twice. If yes return True.
<br>
<I> Link: [Problem 3] (https://github.com/pdesai878/Regexes/blob/8196cba8b49779fc4f25e2ed7510692087c2ff24/Exercises/3.ipynb)</I>
<hr>


   ==> The question mark <b>?</b> rq checks if 0 or more occurence of char specified before ? exists or not.
   <pre>
   eg. print(re.search(r"o?il","coil"))
       o/p: re.Match object; span=(1, 4), match='oil'
       
   eg. print(re.search(r"o?il","cooil"))
       o/p: re.Match object; span=(2, 5), match='oil'
       
   eg. print(re.search(r"oo?il","cooil"))
       o/p: re.Match object; span=(1, 5), match='ooil'
       
   eg. print(re.search(r"o?i","cooil"))
       o/p: re.Match object; span=(2, 4), match='oi'
       
   #when character or characters before ? not present in the string
   eg. print(re.search(r"z?il","cooil"))
       o/p: re.Match object; span=(3, 5), match='il'
    </pre>
<hr>
<h2> Escaping Characters</h2> 
==> What can we do if we need our pattern to match one of the special characters?<br>
   &nbsp- eg.  if we wanted to check if a certain string contains a dot '.' as a part of it<br>
       &nbsp &nbsp - let's try<br>
        &nbsp &nbsp &nbsp print(re.search(r".com","welcome"))<br>
        &nbsp &nbsp &nbsp o/p: re.Match object; span=(2,6), match='lcom' <br>
        &nbsp &nbsp &nbsp &nbsp &nbsp | <BR>
        &nbsp &nbsp &nbsp &nbsp &nbsp | <BR>
        BUT THAT'S NOT WHAT WE WANTED! RIGHT?<BR>
        &nbsp &nbsp &nbsp &nbsp &nbsp | <BR>
        &nbsp &nbsp &nbsp &nbsp &nbsp | <BR>
  ==> To match the actual dot, we need to use an escape character, backslash '\' 
<pre>
    eg. print(re.search(r"\.com","Welcome"))
        o/p: None
    eg. print(re.search(r"\.com","mydomain.com"))
        o/p: re.Match object; span=(8, 12), match='.com'
</pre>
<hr>
  
| - When we see pattern that includes a backslash, it could be escaping a special regex character or a special string character.  <br><br>HOW IS THE CONFUSION AVOIDED??<br>-> by using raw string, '\' backslash would then be interpreted as parsing the regex |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
  <hr>
  
|🔆 Python also uses backslash for a few special sequences that we can use to represent predefined set of characters:<br> |
|-----------------------------------------------------------------------------------------------------------------------|
 <pre>
 1) <b>\w</b> -> matches any alphanumeric character including letters, numbers and underscores [ UNTILL SPACE WHEN USED WITH * rq]
 
              eg.  print(re.search(r"\w*","This is a sentence"))
                   o/p: re.Match object; span=(0, 4), match='This'

              eg.  print(re.search(r"\w*","This_is_a_sentence"))
                   o/p: re.Match object; span=(0, 18), match='This_is_a_sentence'
         
 2) <b>\d</b> -> for matching digits.
 
 3) <b>\s</b> -> for matching white space characters like space, tab or newline.
 
 4) <b>\b</b> -> word boundaries.
 
 </pre>
    
<hr>
  <b>Excercise: </b>Check if the text passed has atleast two groups of alphanumeric characters separated by one or more whitespace.
<br>
<I> Link: [Problem 4] (https://github.com/pdesai878/Regexes/blob/8196cba8b49779fc4f25e2ed7510692087c2ff24/Exercises/4.ipynb)</I>
<hr>
  <br>
  <h2> REGULAR EXPRESSIONS IN ACTION </h2>
  <h3> TASK </h3>
  <p> You have a list of countries and you want to check which of those countries name start and end with an 'a'.</p>
  <p> ==> Pattern: "A.*a" </p>
      
      eg. print(re.search(r"A.*a","Argentina"))
          o/p: re.Match object; span=(0, 9), match='Argentina'
      
      eg. print(re.search(r"A.*a","Azerbaijan"))
          o/p: re.Match object; span=(0, 9), match='Azerbaija'
                              |
                              |
                              |
                             \_/ 
            This was quite surprising. You might have expected it to return None.
            -> This happened becaz we didn't specify that we wanted our pattern to match the whole string.
                              |
                              |
                              |
                             \_/
             SOLUTION: We need to make our pattern more <b> stricter </b>.
                                                                |
                                                                |__ By adding beginning of a line and ending of a line characters in our pattern.
                                                                i.e <b> ^ and & </b>
  
  
==> print(re.search(r"^A.*a$","Azerbaijan")) <br>
    o/p: None ✅ 
  
<hr>
  <b>Excercise: </b>Check whether the string is a valid variable name in Python.
<br>
<I> Link: [Problem 5] (https://github.com/pdesai878/Regexes/blob/8196cba8b49779fc4f25e2ed7510692087c2ff24/Exercises/5.ipynb)</I>
<hr>
  <b>Excercise: </b>Check if the text passed starts with an Uppercase letter, followed by atleast some lowercase letters or space, and ends with a period, question mark or an exclamation point.
<br>
<I> Link: [Problem 6] (https://github.com/pdesai878/Regexes/blob/8196cba8b49779fc4f25e2ed7510692087c2ff24/Exercises/6.ipynb)</I>
<hr>
<b>Excercise: </b>Check if the text passed qualifies as a top level web address.
<br>
<I> Link: [Problem 7] (https://github.com/pdesai878/Regexes/blob/8196cba8b49779fc4f25e2ed7510692087c2ff24/Exercises/7.ipynb)</I>
<hr>
 <b>Excercise: </b>Check time format of a 12hr clock.
<br>
<I> Link: [Problem 8] (https://github.com/pdesai878/Regexes/blob/8196cba8b49779fc4f25e2ed7510692087c2ff24/Exercises/8.ipynb)</I>
<hr>
 <b>Excercise: </b>Check for presence of 2 or more characters or digits surrounded by parenthesis, with atleast first character in upper case(if its a letter).
<I> Link: [Problem 9] (https://github.com/pdesai878/Regexes/blob/8196cba8b49779fc4f25e2ed7510692087c2ff24/Exercises/9.ipynb)</I>
<hr>
<b>Excercise: </b>Check if the text passed includes a possible US zip code.
<br>
<I> Link: [Problem 10] (https://github.com/pdesai878/Regexes/blob/8196cba8b49779fc4f25e2ed7510692087c2ff24/Exercises/10.ipynb)</I>
<hr>
<br>
  <H2> ADVANCED REGULAR EXPRESSIONS </H2>
=> Till now we'hv used search function to check if a string matched a certain pattern. But the only thing we did with the result is print. <br>
=> Printing is useful when we want to see if a string matches a certain pattern. But, most of the times, we want to take the information out of the result and use it for something else.
  
<br>
  for eg. We may want to extract the hostname or the process id from a log file and use that value for another operation. For that you need to use the concept of Regular Expressions called <b> Capturing Groups</b>
<br>
<br>

 Capturing groups are portions of the pattern that are enclosed in parenthesis.
 
 -> Let's say we have a list of peoples fullname. These names are stored as lastname comma firstname.
    We want to turn this around and create a string that starts with firstname followed by lastname.
  <pre>
result=re.search(r"^(\w*),(\w*)$","Desai,Parth")
print(result)
o/p: re.Match object; span=(0, 11), match='Desai,Parth'
                          |
                          |
                          |
                         \_/                      
# The match object has more attributes and methods than the ones shown by print.
                          | 
                          |
                          |
                         \_/
=> groups() function:  returns a tuple.
   eg. print(result.groups())
       o/p: ('Desai', 'Parth')
   
   We can also use indexing to access these groups.
    print(result[0])
    o/p: Desai,Parth
    
    print(result[1])
    Desai
    
    print(result[2])
    o/p: Parth
           
</pre>
<hr>
  <b>Excercise: </b> Create a function that would do the rearranging of name. i.e lastname comma firstname --> firstname lastname.
<br>
<I> Link: [Problem 11] (https://github.com/pdesai878/Regexes/blob/8196cba8b49779fc4f25e2ed7510692087c2ff24/Exercises/11.ipynb)</I>
<hr>
  
  <h2> More on Repitition Qualifiers</h2>
  => Upto now, we'hv used the * + and ? repitition qualifiers. <br>
  => What if we wanted a pattern that repeats a specific number of times?? <br>
  &nbsp &nbsp -  This could happen if we're processing a line that we know has some specific data in a column, or we know that we want a string of &nbsp &nbsp a specific length.
  <br>
  &nbsp &nbsp - We could manually write the same pattern as many times as we need it but, it would be hard to read and hard to maintain.<br><br>
| <b>That's why Python also offers numeric repitition qualifiers. These are written between curly brackets and can be one or two numbers specifying a range </b> |
<br>

<pre>
eg. To match atleast 5 lettered word/string from the given text
    print(re.search(r"[a-zA-Z]{5}","A Ghost"))
    o/p: re.Match object; span=(2, 7), match='Ghost'
  
eg. What if we have more matches for our search?
    print(re.search(r"[a-zA-Z]{5}","A scary ghost appeared"))
    o/p: re.Match object; span=(2, 7), match='scary'         #we get the firstmatch
                          | 
                          |
                          |
                         \_/
    #you can use findall function for getting all matches.
     eg. print(re.findall(r"[a-zA-Z]{5}","A scary ghost appeared"))
         o/p: ['scary', 'ghost', 'appea']
                                   | 
                                   |_ now, we have an extra match for a word thats actually longer.
                                   
eg. To match exactly 5 lettered word/string from the given text.
    -> We can do this using \b, which matches the word limits at the beginning and end of the pattern to indicate that we want full words.
    
    print(re.findall(r"\b[a-zA-Z]{5}\b","A scary ghost appeared"))
    o/p: ['scary', 'ghost']
</pre>
<br>

<pre>
    
# We can also use 2 numbers for the range.

eg. if we wanted to match a range of five to ten letters/numbers, we could use an expression:
    print(re.findall(r"\w{5,10}","I really like strawberries"))
    o/p: ['really', 'strawberri']
    
    
# These ranges can also be open ended. A number followed by a comma means that atleast that many repititions with no upper boundary, limited only by the maximum repititions in the source text.

eg. print(re.findall(r"\w{5,}","I really like strawberries"))
    o/p: ['really', 'strawberries']
    
    
 # A comma followed by a number means from zero upto that amount of repititions.
 eg. print(re.search(r"s\w{,20}","I really like strawberries"))
     o/p: re.Match object; span=(14, 26), match='strawberries'
     
</pre>
  
<hr>
  <h2> Extracting a PID using regexes</h2>
  
<pre>
log="July 31 07:51:48 mycomputer bad_process [12345]: ERROR Performing package upgrade"
regex="\[(\d*)\]"              #observe the use of escape character, capturing group, special sequence, repitition qualifier
result=re.search(regex,log)
print(result[1])               

o/p: 12345
                        | 
                        |
                        |
                       \_/
  #After calling the search function, since we know that we have capturing group in our expression, we can access the matching data by accessing the matching data at index 1.
  
  What if our text/string didn't really have a block of numbers between square brackets?
  eg. result=re.search(r"\[(\d*)\]","99 elephants in the [cage]")
      print(result[1])
            |
            |
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'NoneType' object is not subscriptable

      ->REASON FOR ERROR: we tried to access index 1 of result which was None.
  
</pre>

<hr>
<b>Excercise: </b> Create a function that would extract the PID when possible and returns something else if not possible.
<br>
<I> Link: [Problem 12] (https://github.com/pdesai878/Regexes/blob/8196cba8b49779fc4f25e2ed7510692087c2ff24/Exercises/12.ipynb)</I>
<hr>
<h2> Splitting and Replacing</h2>

<b> split() function of the re module</b> => It works similarly to the split function we use with strings. But, instead of taking string as a separator, we take any regular expression as a separator. <br>
<pre>

eg. Task: We may want to split a piece of text into different sentences. To do that we not only need to check for dots but also for question mark or exclamation mark since they're also valid sentence endings.

re.split(r"[.?!]","One sentence. Another one? And the last one!") 
o/p: ['One sentence', ' Another one', ' And the last one', '']
                        | 
                        |
                        |
                        > Checkout how we are not escaping the characters that we wrote inside the square brackets. Thats becaz anything thats inside the square
                          brackets is taken for as a literal character and not for as a special character.
                        > Also see how the notation marks are not in the resulting list.
                        | 
                        |
                        |                        
# If we want our split list to include the elements that we're using to split, we can use <b> Capturing parenthesis</b>

  re.split(r"([.?!])","One sentence. Another one? And the last one!") 
  o/p: ['One sentence', '.', ' Another one', '?', ' And the last one', '!', '']
</pre>
<br>
<b> sub() function of the re module</b> => It works similarly to the replace function we use with strings. Here sub() uses regular expression for both matching and replacing.
<br>
<pre>
eg. Lets say we had some logs in our system that included email addresses of users and we wanted to anonymize the data by removing all the addresses. Yes, we can do that by using the sub()

    re.sub(r"[\w.%+-]+@[\w.-]+","[REDACTED]","Received an email from parthmanishdesai@gmail.com")
    o/p: 'Received an email from [REDACTED]'


eg. Lets now look at an example using sub where we use regex for replacing.
    Task: switch the order of names of people and use sub() to create the new string.
    
    re.sub(r"^([\w.-]*),([\w.-]*)$",r"\2 \1","DESAI,PARTH")
    o/p: 'PARTH DESAI'
                                        | 
                                        |__ here \2 indicates the 2nd capture group and \1 indicates the 1st capture group.
</pre>

<hr>
<b>Excercise: </b> Split a piece of text either by letter 'a' or by the word 'the'.
<br>
<I> Link: [Problem 13] (https://github.com/pdesai878/Regexes/blob/8196cba8b49779fc4f25e2ed7510692087c2ff24/Exercises/13.ipynb)</I>
<hr>

DO CHECK OUT: <br>
[Problem 14] (https://github.com/pdesai878/Regexes/blob/8196cba8b49779fc4f25e2ed7510692087c2ff24/Exercises/14.ipynb) <br>
[Problem 15] (https://github.com/pdesai878/Regexes/blob/8196cba8b49779fc4f25e2ed7510692087c2ff24/Exercises/15.ipynb) <br>
[Problem 16] (https://github.com/pdesai878/Regexes/blob/8196cba8b49779fc4f25e2ed7510692087c2ff24/Exercises/16.ipynb)

<hr>
<hr>
                                        



    
   
                                   
   
  

                                   
    


  
  
    
    

    

      
      

    
  
  
 



       
       
  
              
    
                
    
    

      
      
     



  

                 
 

   
   

          
          
          
          
        
   
