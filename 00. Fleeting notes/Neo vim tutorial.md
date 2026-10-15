


---


MOVEMENT:
	
	j - down    (looks like arrow down)
	k - up
	
	
	h -left
	l- right

TEXT DELETION:
x -  used to delete the character on which the cursor is on.
a, i - insert text after the cursor, insert text before the cursor.

u - undo the changes

dw - used to delete a word from the cursor till start of second word (de- can be used to delete before start od character.)
d$ - delets whole line from the cursor onwards.



MOTION WITH NUMBERS:
	2e - will move the cursor at the end of second word.
	2w - will move the cursor at the start of 3rd word.
	0 - will move the cursor at atart of line.
	$ - to move to the end of the line.


DELETING THE WORD WITH MOTION:
	d2e - deletes two words in font of the cursor
	d2w - deletes two words unitll third word in frot of the cursor


 OPERATING LINESS:
	 dd - deletes whole line.


UNDO AND REDO:
	U - UNOD
	ctrl + r - redo


- [x] Start with lesson 3

PUT COMMAND
P - move the whole line which has been cut before using dd.

rx - used r to replace the character which is under the cursor with any letter typed afterwards.kjkk

ce - remove the whole word and start writing from the current word snds
c5e- removes 5 words in front of cursor an then puts in insett mode.


- [ ] Start with lesson 4



crtl + g - check thr file status and location and number if lines
4 -G - moves to the 4th line (presss any nunber)

G - cursor will move to buttom of the file
gg - cursor will move to the top of the file


 SEARCHING:
/(word) - will search word
	n - next word
	N - previous word
? - seach word in backward direction.

% - when cursor is placed on one of the ( thenby pressing % we move to the other )


SUBSTITUTING:
:S/OLD/NEW - where old is old word and new is new word in the current line.

:S/OLD/NEW/g - change all teh occurances of old in current line

:#,#S/OLD/NEW/g - between two line where # is line number

:%S/OLD/NEW/g - chnage old in whole file.


	
v - is used to select the text.
o - add a line above the cursor.
O - add a line below the cursor.

y - copies the text
