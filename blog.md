# day 6
today I worked more on NBasic, which is pretty much done at this point.

all the instructions are done and fleshed out, and the documentation is written.

all i need to do is integrate it into NLOS, which i will do on day 7.
so for right now i will be taking a break from coding.

i also installed a new font, fira code.  
it has ligatures, so `//=` looks really cool, and `<=` actually looks like `≤`  
i am really happy with it, and i have put it in all my ides.  

# day 5
so over the weekend i didnt do anything, so i skipped those days.

i am currently working on NBasic, so stay tuned for updates on NLOS.

currently i have gotten this:
```py
def runbas(code):
    code = code.split("\n")
    variables = {}
    i = 0
    while i < len(code):
        line = code[i].split()
        if line[0] == "jmp":
            try:
                jump_to = int(line[1]) - 1
                if jump_to < 0 or jump_to >= len(code):
                    raise ValueError("Jump target out of range")
                i = jump_to
            except ValueError as e:
                print(f"Error: {e}")
                return
        elif line[0] == "var":
            if len(line) < 2:
                print("Error: Invalid variable declaration")
                return
            variables[line[1]] = None
        elif line[0] == "io":
            if len(line) < 2:
                print("Error: Invalid IO command")
                return
            if line[1] not in variables:
                print(f"Error: Undefined variable '{line[1]}'")
                return
            print(variables[line[1]])
        elif line[0] == "ios":
            print(" ".join(line[1:]))
        elif line[0] == "set":
            if len(line) < 3:
                print("Error: Invalid set command")
                return
            if line[1] not in variables:
                print(f"Error: Undefined variable '{line[1]}'")
                return
            try:
                variables[line[1]] = eval(line[2])
            except Exception as e:
                print(f"Error: {e}")
                return
        elif line[0] == "jif":
            if len(line) < 5:
                print("Error: Invalid jif command")
                return
            if line[1] not in variables or line[3] not in variables:
                print(f"Error: Undefined variable in jif command")
                return
            var1 = variables[line[1]]
            var2 = variables[line[3]]
            try:
                jump_to = int(line[4]) - 1
                if jump_to < 0 or jump_to >= len(code):
                    raise ValueError("Jump target out of range")
                if (line[2] == "<" and var1 < var2) or (line[2] == ">" and var1 > var2) or (line[2] == "=" and var1 == var2):
                    i = jump_to
            except ValueError as e:
                print(f"Error: {e}")
                return
        elif line[0] == "in":
            if len(line) < 2:
                print("Error: Invalid input command")
                return
            variables[line[1]] = int(input("Enter a number: "))
        i += 1
```

its not pretty but its a start. once i get it completly managed, things will be implemented.

# day 2
ok so i got some code.
i can code in cpp now idk how tho
have the crappiest hello world i could manage
```cpp
#include <iostream>
using namespace std;

int main()
{
    cout << "i can code in c++ now?\nwierd\n" << flush;
    unsigned long i = 0;
    i--;
    cout << i << "\n";
    string d = "";
    cin >> d;
    cout << d << flush;
    return 0;
}
```
basic stuff for messing with cpp and unsigned integers.

im going back to c#, as i know how to code in it, plus it is actually intuitive to use

plus, i can actually clear the screen without having to be using things like "ansi" or "#ifdef"   
srsly i hate #ifdef.  
you c++ ers can fight me on this

visual studio borke, so i needed to reinstall the .net framework

i may be getting it to work soon

just realized c# has more imports than a python script for an ai

```cs
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace nlttysecs
{
    internal class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("im pretty sure it works now");
            Console.ReadKey();
        }
    }
}
```

update: i figured out functions
```cs
using System;
using System.Collections.Generic;
using System.Linq;
using System.Runtime.InteropServices;
using System.Text;
using System.Threading.Tasks;

namespace nlttysecs
{
    internal class Program
    {
        static void print(params string[] strings) { 
            foreach (string s in strings)
            {
                Console.Write(s + " ");
            }
        }
        static void Main(string[] args)
        {
            // var scnmem = new List<Tuple<int, int, string>> [1, 1, 1];
            print("A");
            Console.ReadKey();
        }
    }
}
```

im gonna go play people playground, thats enough headache for now

another thing: you guys asked for my game so i added it.  
Have fun playing explora!

# day 1
ok i figured out how to use a blog  
so umm yeah blog cool

anyways so i made this guthib repo called NLTTYSE  
its called that cause i was too lazy to learn ansi escape codes, so i remade them but ~~more complicated~~ simpler.  
its literally one file, so i can just put it here:
```py
"""
A library for emulating a screen in a terminal using letterpixels.
Letterpixels are not pixels in the normal sense, only letters in a grid acting as such.

scnmem: The screen memory
sx: The screen's length in letterpixels
sy: the screen's height in letterpixels
"""

scnmem = []
sx = 64
sy = 32
import os
import keyboard
def cls():
    """Clears the terminal on most systems."""
    print("\033[H\033[J")
    if os.name == "nt":
        os.system("cls")
    else:
        os.system("clear")
def pushp(x, y, t="\u2588", r=True):
    """Pushes a pixel to the screen."""
    global scnmem
    popp(x, y)
    scnmem += [[x, y, t]]
    if r:
        updscn()
def clrscn():
    """Clears the screen memory. Requires an update afterwards for compatibility."""
    global scnmem
    scnmem = []
def peekp(x, y, pn=False):
    """Grabs what pixel is at (x, y) on the screen. If pn (preserve null) is on, gives None when there is no pixel there."""
    global scnmem
    for i in scnmem:
        if i[0] == x and i[1] == y:
            return i[2]
    if pn:
        return None
    else:
        return " "
def pescnmem(x,y):
    """Checks if a pixel exists."""
    global scnmem
    for i in scnmem:
        if i[0] == x and i[1] == y:
            return True
    return False
def popp(x,y):
    """Pops a pixel from the screen at the specified position."""
    global scnmem
    for i in scnmem:
        if i[0] == x and i[1] == y:
            scnmem.remove(i)
def updscn():
    """Updates the screen."""
    cls()
    global scnmem
    l = ""
    for y in range(0, sx):
        for x in range(0, sy):
            if pescnmem(x, y):
                d = peekp(x, y)
                l += d
            else:
                l += " "
        l += "\n"
    print(l)
def pusht(x, y, t, u = True):
    """Pushes text to the screen. \"u\" is the update flag."""
    lt = list(t)
    h = 0
    for rt in lt:
        pushp(x+h, y, rt, False)
        h+=1
    if u:
        updscn()
def getkey():
    """Basically a wrapper for \"keyboard.readkey()\""""
    return keyboard.read_key(True)
def kscnpipe(x, y, l=64):
    """Handles textboxes. \"l\" is the length of the textbox."""
    apop(x, y, l)
    updscn()
    stop = False
    offset = 0
    while not stop:
        k = getkey()
        if k == "enter":
            stop = True
            break
        elif k == "space":
            pushp(x+offset, y, " ")
            offset += 1
            offset = abs(offset)
        elif k == "backspace":
            offset -= 1
            offset = abs(offset)
            popp(x+offset, y)
            updscn()
        elif len(k) != 1:
            pass
        else:
            pushp(x+offset, y, k)
            offset += 1
            offset = abs(offset)
        if offset >= l:
            return
def apeek(x, y, le, pn):
    """Peeks an area of the screen. Good for getting what the user typed using \"kscnpipe()\". PN flag provided for compatiblity."""
    out = ""
    for i in range(le):
        out += peekp(x+i, y)
    return out
def apop(x, y, le):
    """Pops an area from the screen."""
    pusht(x, y, " "*le)
```

it is a bit large, after the first few iterations it required being a file instead of a message to go on discord  
honestly i think this is the best thing i have made, its really cool and i use it almost constantly

it requires only keyboard, and i was going to move it to c code and learn c, but idk if i have the patience for learning c programs  
~~expect c code or related in my blog way later in the future~~

addendium: guess whos pulling out visual studio right now at 1675307363 unix time to learn c? thats right, me.  
ok so windows hates c for some reason, so im ~~doing meth~~ learning c++ instead

update:
![image](https://user-images.githubusercontent.com/83925246/216225054-8098d59d-eb85-4f5a-b3d5-9dc8db3df39d.png)
