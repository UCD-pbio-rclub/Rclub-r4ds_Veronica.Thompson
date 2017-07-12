# Strings1
Veronica  
7/11/2017  



## 14. Strings

### 14.2 String basics

**1. In code that doesn’t use stringr, you’ll often see paste() and paste0(). What’s the difference between the two functions? What stringr function are they equivalent to? How do the functions differ in their handling of NA?**


```r
x <- c("a", "b", "c")
y <- c("1", "2", "3")
paste(x, y)
```

```
## [1] "a 1" "b 2" "c 3"
```

```r
paste0(x, y)
```

```
## [1] "a1" "b2" "c3"
```

```r
str_c(x, y)
```

```
## [1] "a1" "b2" "c3"
```

paste() has a default separation of space, while paste0() has no separation. str_c() is similar to paste0() in that it as no default separation

**2. In your own words, describe the difference between the sep and collapse arguments to str_c().**


```r
str_c(x, y, sep = "-")
```

```
## [1] "a-1" "b-2" "c-3"
```

```r
str_c(x, y, sep = "-", collapse = "...")
```

```
## [1] "a-1...b-2...c-3"
```

sep indicates what character will separate the characters that are being combined. collapse indicates the separator between characters in a string. When collapse does not equal NULL the output string will have a length of one. 

**3. Use str_length() and str_sub() to extract the middle character from a string. What will you do if the string has an even number of characters?**


```r
odd <- "1234567"
str_length(odd)
```

```
## [1] 7
```

```r
str_sub(odd, str_length(odd)/2, str_length(odd)/2)
```

```
## [1] "3"
```

```r
str_sub(odd, str_length(odd)/2+.5, str_length(odd)/2+.5)
```

```
## [1] "4"
```

```r
even <- "123456"
str_length(even)
```

```
## [1] 6
```

```r
str_sub(even, str_length(even)/2+.5, str_length(even)/2+.5)
```

```
## [1] "3"
```

```r
#this code prints the character before the "middle"
```

**4. What does str_wrap() do? When might you want to use it?**


```r
thanks_path <- file.path(R.home("doc"), "THANKS")
thanks <- str_c(readLines(thanks_path), collapse = "\n")
thanks <- word(thanks, 1, 3, fixed("\n\n"))
cat(str_wrap(thanks), "\n")
```

```
## R would not be what it is today without the invaluable help of these people,
## who contributed by donating code, bug fixes and documentation: Valerio Aimale,
## Thomas Baier, Henrik Bengtsson, Roger Bivand, Ben Bolker, David Brahm, G"oran
## Brostr"om, Patrick Burns, Vince Carey, Saikat DebRoy, Matt Dowle, Brian D'Urso,
## Lyndon Drake, Dirk Eddelbuettel, Claus Ekstrom, Sebastian Fischmeister, John
## Fox, Paul Gilbert, Yu Gong, Gabor Grothendieck, Frank E Harrell Jr, Torsten
## Hothorn, Robert King, Kjetil Kjernsmo, Roger Koenker, Philippe Lambert, Jan
## de Leeuw, Jim Lindsey, Patrick Lindsey, Catherine Loader, Gordon Maclean, John
## Maindonald, David Meyer, Ei-ji Nakama, Jens Oehlschaegel, Steve Oncley, Richard
## O'Keefe, Hubert Palme, Roger D. Peng, Jose' C. Pinheiro, Tony Plate, Anthony
## Rossini, Jonathan Rougier, Petr Savicky, Guenther Sawitzki, Marc Schwartz, Arun
## Srinivasan, Detlef Steuer, Bill Simpson, Gordon Smyth, Adrian Trapletti, Terry
## Therneau, Rolf Turner, Bill Venables, Gregory R. Warnes, Andreas Weingessel,
## Morten Welinder, James Wettenhall, Simon Wood, and Achim Zeileis. Others have
## written code that has been adopted by R and is acknowledged in the code files,
## including
```

```r
cat(str_wrap(thanks, width = 40), "\n")
```

```
## R would not be what it is today
## without the invaluable help of these
## people, who contributed by donating
## code, bug fixes and documentation:
## Valerio Aimale, Thomas Baier, Henrik
## Bengtsson, Roger Bivand, Ben Bolker,
## David Brahm, G"oran Brostr"om, Patrick
## Burns, Vince Carey, Saikat DebRoy,
## Matt Dowle, Brian D'Urso, Lyndon Drake,
## Dirk Eddelbuettel, Claus Ekstrom,
## Sebastian Fischmeister, John Fox, Paul
## Gilbert, Yu Gong, Gabor Grothendieck,
## Frank E Harrell Jr, Torsten Hothorn,
## Robert King, Kjetil Kjernsmo, Roger
## Koenker, Philippe Lambert, Jan de
## Leeuw, Jim Lindsey, Patrick Lindsey,
## Catherine Loader, Gordon Maclean, John
## Maindonald, David Meyer, Ei-ji Nakama,
## Jens Oehlschaegel, Steve Oncley, Richard
## O'Keefe, Hubert Palme, Roger D. Peng,
## Jose' C. Pinheiro, Tony Plate, Anthony
## Rossini, Jonathan Rougier, Petr Savicky,
## Guenther Sawitzki, Marc Schwartz, Arun
## Srinivasan, Detlef Steuer, Bill Simpson,
## Gordon Smyth, Adrian Trapletti, Terry
## Therneau, Rolf Turner, Bill Venables,
## Gregory R. Warnes, Andreas Weingessel,
## Morten Welinder, James Wettenhall, Simon
## Wood, and Achim Zeileis. Others have
## written code that has been adopted by R
## and is acknowledged in the code files,
## including
```

```r
cat(str_wrap(thanks, width = 60, indent = 2), "\n")
```

```
##   R would not be what it is today without the invaluable
## help of these people, who contributed by donating code,
## bug fixes and documentation: Valerio Aimale, Thomas Baier,
## Henrik Bengtsson, Roger Bivand, Ben Bolker, David Brahm,
## G"oran Brostr"om, Patrick Burns, Vince Carey, Saikat
## DebRoy, Matt Dowle, Brian D'Urso, Lyndon Drake, Dirk
## Eddelbuettel, Claus Ekstrom, Sebastian Fischmeister, John
## Fox, Paul Gilbert, Yu Gong, Gabor Grothendieck, Frank E
## Harrell Jr, Torsten Hothorn, Robert King, Kjetil Kjernsmo,
## Roger Koenker, Philippe Lambert, Jan de Leeuw, Jim Lindsey,
## Patrick Lindsey, Catherine Loader, Gordon Maclean, John
## Maindonald, David Meyer, Ei-ji Nakama, Jens Oehlschaegel,
## Steve Oncley, Richard O'Keefe, Hubert Palme, Roger D. Peng,
## Jose' C. Pinheiro, Tony Plate, Anthony Rossini, Jonathan
## Rougier, Petr Savicky, Guenther Sawitzki, Marc Schwartz,
## Arun Srinivasan, Detlef Steuer, Bill Simpson, Gordon
## Smyth, Adrian Trapletti, Terry Therneau, Rolf Turner, Bill
## Venables, Gregory R. Warnes, Andreas Weingessel, Morten
## Welinder, James Wettenhall, Simon Wood, and Achim Zeileis.
## Others have written code that has been adopted by R and is
## acknowledged in the code files, including
```

```r
cat(str_wrap(thanks, width = 60, exdent = 2), "\n")
```

```
## R would not be what it is today without the invaluable
##   help of these people, who contributed by donating code,
##   bug fixes and documentation: Valerio Aimale, Thomas Baier,
##   Henrik Bengtsson, Roger Bivand, Ben Bolker, David Brahm,
##   G"oran Brostr"om, Patrick Burns, Vince Carey, Saikat
##   DebRoy, Matt Dowle, Brian D'Urso, Lyndon Drake, Dirk
##   Eddelbuettel, Claus Ekstrom, Sebastian Fischmeister, John
##   Fox, Paul Gilbert, Yu Gong, Gabor Grothendieck, Frank E
##   Harrell Jr, Torsten Hothorn, Robert King, Kjetil Kjernsmo,
##   Roger Koenker, Philippe Lambert, Jan de Leeuw, Jim Lindsey,
##   Patrick Lindsey, Catherine Loader, Gordon Maclean, John
##   Maindonald, David Meyer, Ei-ji Nakama, Jens Oehlschaegel,
##   Steve Oncley, Richard O'Keefe, Hubert Palme, Roger D. Peng,
##   Jose' C. Pinheiro, Tony Plate, Anthony Rossini, Jonathan
##   Rougier, Petr Savicky, Guenther Sawitzki, Marc Schwartz,
##   Arun Srinivasan, Detlef Steuer, Bill Simpson, Gordon
##   Smyth, Adrian Trapletti, Terry Therneau, Rolf Turner, Bill
##   Venables, Gregory R. Warnes, Andreas Weingessel, Morten
##   Welinder, James Wettenhall, Simon Wood, and Achim Zeileis.
##   Others have written code that has been adopted by R and is
##   acknowledged in the code files, including
```

```r
cat(str_wrap(thanks, width = 0, exdent = 2), "\n")
```

```
## R
##   would
##   not
##   be
##   what
##   it
##   is
##   today
##   without
##   the
##   invaluable
##   help
##   of
##   these
##   people,
##   who
##   contributed
##   by
##   donating
##   code,
##   bug
##   fixes
##   and
##   documentation:
##   Valerio
##   Aimale,
##   Thomas
##   Baier,
##   Henrik
##   Bengtsson,
##   Roger
##   Bivand,
##   Ben
##   Bolker,
##   David
##   Brahm,
##   G"oran
##   Brostr"om,
##   Patrick
##   Burns,
##   Vince
##   Carey,
##   Saikat
##   DebRoy,
##   Matt
##   Dowle,
##   Brian
##   D'Urso,
##   Lyndon
##   Drake,
##   Dirk
##   Eddelbuettel,
##   Claus
##   Ekstrom,
##   Sebastian
##   Fischmeister,
##   John
##   Fox,
##   Paul
##   Gilbert,
##   Yu
##   Gong,
##   Gabor
##   Grothendieck,
##   Frank
##   E
##   Harrell
##   Jr,
##   Torsten
##   Hothorn,
##   Robert
##   King,
##   Kjetil
##   Kjernsmo,
##   Roger
##   Koenker,
##   Philippe
##   Lambert,
##   Jan
##   de
##   Leeuw,
##   Jim
##   Lindsey,
##   Patrick
##   Lindsey,
##   Catherine
##   Loader,
##   Gordon
##   Maclean,
##   John
##   Maindonald,
##   David
##   Meyer,
##   Ei-
##   ji
##   Nakama,
##   Jens
##   Oehlschaegel,
##   Steve
##   Oncley,
##   Richard
##   O'Keefe,
##   Hubert
##   Palme,
##   Roger
##   D.
##   Peng,
##   Jose'
##   C.
##   Pinheiro,
##   Tony
##   Plate,
##   Anthony
##   Rossini,
##   Jonathan
##   Rougier,
##   Petr
##   Savicky,
##   Guenther
##   Sawitzki,
##   Marc
##   Schwartz,
##   Arun
##   Srinivasan,
##   Detlef
##   Steuer,
##   Bill
##   Simpson,
##   Gordon
##   Smyth,
##   Adrian
##   Trapletti,
##   Terry
##   Therneau,
##   Rolf
##   Turner,
##   Bill
##   Venables,
##   Gregory
##   R.
##   Warnes,
##   Andreas
##   Weingessel,
##   Morten
##   Welinder,
##   James
##   Wettenhall,
##   Simon
##   Wood,
##   and
##   Achim
##   Zeileis.
##   Others
##   have
##   written
##   code
##   that
##   has
##   been
##   adopted
##   by
##   R
##   and
##   is
##   acknowledged
##   in
##   the
##   code
##   files,
##   including
```

str_wrap() reformats size of the space in which a string is written. This might be helpful for formating when building a webpage.

**5. What does str_trim() do? What’s the opposite of str_trim()?**

str_trim() removes the white space from either side of a string. str_pad() can be used to add whitespace.

**6. Write a function that turns (e.g.) a vector c("a", "b", "c") into the string a, b, and c. Think carefully about what it should do if given a vector of length 0, 1, or 2.**

????

### RegexOne  
#### Tutorial 

1. abc  
2. 123  
3. ...\.  
4. [cmf]an  
5. [^b]og  
6. [A-C]\w\w  
7. waz{3,5}up  
8. a+\w+  
9. \d* files? found\?  
10. \d.\s+abc  
11. ^M\w*: \w*  
12. ^(file_\w+).pdf  
13. ^(\w+ (\d+))$  
14. (\d{4})x(\d{3,4})  
15. I love (cats|dogs)  
16. .+\.$   

#### Matching decimal numbers

1. ^\-?\d*.?\d*.*\d+$  
2. (\d{3})  
3. (^\w+\.?\w+)\+?\w*\@\S+  
4. (\w*)\>$ (not full match of line)  
5. (\w+)\.(jpg|png|gif)$  
6. ^\s*(.*)$  
7. (\w+)\((\w+\.\w+)\:(\d+)\)$  
8. (^\w+)\:\/\/(\w+\.?\-?\w*\.?\w*)\:?(\d*)  

#### 14.3.5 Exercises

**1. Describe, in words, what these expressions will match:**  

**(.)\1\1**  
Any character that is repeated three times in a row. ex: aaa

**"(.)(.)\\2\\1"**  
a palindrome with the length of 4. ex: anna

**(..)\1**  
a pair of characters repeated twice. ex: nana

**"(.).\\1.\\1"**  
Two characters, followed by the first character twice. ex: 

**"(.)(.)(.).*\\3\\2\\1"**  
a sequence of three characters, zero to any number of other characters, the original three characters in inverted order ex: abcdhdhdhdhdhdcda or abccda

**2. Construct regular expressions to match words that:**

**Start and end with the same character.**  
(.).*\1

**Contain a repeated pair of letters (e.g. “church” contains “ch” repeated twice.)**  
.*(..).*\1.*

**Contain one letter repeated in at least three places (e.g. “eleven” contains three “e”s.)**
.*(.).*\1.*\1.*
