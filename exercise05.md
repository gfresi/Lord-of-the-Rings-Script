## Task 1

**"lotrscripts.csv"** is a table file containing the script of the 3 *"The Lord of the Rings"* movies.  

The file is structured as follows:
1. In the column on the left is the number of lines, followed by 
2. the name of the character who pronounces them. (all written in Upper Case)
3. Then the quoted Line itself 
4. the Trilogy Movie in which the Line is pronounced.

Problem is that the file is not clean:
- first, we find all the information within a single table when we open it with any Spreadsheet.
-  also, there are discrepancies in the writing of the words:
for example, there are the same words written differently, names written incorrectly, misspellings, etc.
- furthermore, some pieces of information were missing in certain columns.
- Some metacharacters who couldn't be read by Spreadsheet were transformed into weird Symbols.
- Finally there were way too many unuseful Blank Spaces before, in between and after Words.

I'm gonna write in this text how I cleaned the file and my solution to the given Tasks.

## Task 2


     I downloaded the file and opened it up with Excel.
1. I selected the column that contained the whole content of the file, selected then **Data** and the **Convert Text to columns** function.
2. Inside the function I crossed **Delimited** to tell the Program that I wanted to separate the different Fields and chose the *Comma* as Separator.
3. On a blank cell, I wrote a **Trim** function, that deletes all the excess blank spaces before, in between and afterwords (It left only the single Blank Spaces between each word)
4. I applied the function to the Columns **B** **C** and **D** (which contain characters, dialogs, and Movies)
5. As the quote Marks were not read correctly but shown in my spreadsheet as special characters (**¬†**), I used the **Find and Replace** feature in Excel, and Replaced every one of those symbols with blank space.
6. With a first Overview I noticed that some Movie Names were missing from column D, so I googled the given line and wrote the missing information.

       The File looked kind of clean, but I noticed several Mistakes/Misspellings/Discrepancies in the names of the characters.  

- So, to understand what had to be changed I moved to the **Bash** and to the Directory where I stored the file.


1. I used the command **cut -d ';' -f 2 lotr_script_clean.csv | sort | uniq -ci |sort** to sort the different character **unique Names**.  

       Through this command, I told the Bash to search in Column 2, for how many times **unique words** separated through a Semi-colon (**-d ';'**) occur, with no UPPER/lower Case distinction (**uniq -ci**), to merge same Words (**sort**) and finally sort them again from the least occurring to the most.(again **sort**)
       
2. I looked for discrepancies. First, I found Misspellings (*Argorn* instead of *Aragorn*, *Galadril* in place of *Galadriel* ,*Awen* in place of *Arwen* and *Grishnak* in place of *Grishnakh* also one **(Gollum** with a single open Bracket and one *Gan Dalf* with blank space in between)  
Then I found the same words were written differently (*Voiceover* , *Voice-over* and *Voice Over).
3. I went back to Excel and used again the **Find and Replace** Feature to merge this Discrepancies (Replaced every *Argorn* with *Aragorn* .... *Voiceover* and *Voice over* with *Voice-over* etc.)
4. Checked again on The Bash and finally the Names were all merged.

***Those are the steps i took to clean the Data***

## Task 3

1. ## Number of lines and of unique Words: 

### Lines
      
    **cut -f 3 lotr_script_clean.csv| wc -l** 
     
`2390`

  This command made the Program open the 3rd Column **(cut -f 3)** where all the lines of the scripts and count the lines into the output (**wc -l**)  
  
  
 
 ### Numbers 
 
    **cut  -d ';' -f3 lotr_script_clean.csv | tr "[:punct:]" " " |  tr " " "\n"  | sort | uniq -ci | sort**  

`3859`

  This command did the same as the last one in the beginning, but I added the command **tr "[:punct:]" " "** to allow me to search for the words also when they have Punctation attached (ex. **"RUN!"**), **tr " " "\n"** to transform Space Characters into New Lines. **sort** to merge the duplicates, **uniq -ci** to search how many times unique words with no Upper/Lower case distinction and again **sort** to sort them as they occur.
  
2. ## Distribution of the 3 Films:

        **cut -d ';' -f 4 lotr_script_clean.csv | sort | uniq -ci |sort**  

` 507 The Fellowship of the Ring `
` 873 The Return of the King`
`1010 The Two Towers`

  Clear enough: cut in the 4th Column where Movies Titles are words separated by a *semicolon*.
  Then sort them to merge duplicates,**uniq -ci** to print the list of Unique words with no *Upper/Lower* Case distinction and **sort** to rank them


3. ## Top 5 characters in the char column: 
                  
        **cut -d ';' -f 2 lotr_script_clean.csv | sort | uniq -ci |sort**

`163 PIPPIN`
`189 ARAGORN`
`205 GANDALF`
`217 SAM`
`226 FRODO`

I already explained that when I talked about how I cleaned the Data so  I'm just going to quote myself:
"I told the Bash to search in Column 2, for how many times **unique words** separated through a *Semi-colon* (**-d ';'**) occur,*with no UPPER/lower Case distinction* (**uniq -ci**), to merge same Words (**sort**) and finally sort them again from the least occurring to the most.(again **sort**)"


 
4. ## Top 5 characters in the dialogs:

        **cut  -d ';' -f3 lotr_script_clean.csv | tr "[:punct:]" " " |  tr " " "\n" | grep -iE "EOWYN|LEGOLAS|FARAMIR|GOLLUM|THEODEN|GIMLI|PIPPIN|ARAGORN|MERRY|SMEAGOL|SAM|GANDALF|FRODO" | sort | uniq -ci | sort**

  `40 Merry`
  `48 Smeagol`
  `65 Sam`
  `71 GANDALF`
  `147 Frodo`
 
For this last one I **cut**ted from the column **3** ,searched for Words also with punctation signs **( tr "[:punct:]" " " )**,transformed spaces in new lines  **(tr " " "\n")**, then used the **grep** command with the **-iE** option to allow me to look for the alternative words inside the pipe *with no upper/lower case distinction* .  
I used again my command from Task 1 **cut  -d ';' -f3 lotr_script_clean.csv | tr "[:punct:]" " " |  tr " " "\n"  | sort | uniq -ci | sort** that looks for unique words and how often they occur, put between the Pipes in *Task 4* the Names that occurred the most, then merged the duplicates with **sort** , made a list of **uniq** words with no Upper/Lower Case Distinction and ranked them.



