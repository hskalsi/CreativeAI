## Sections

- [Python Classes] (#python-classes)
- [Inheritance] (#python-inheritance)
- [Tuples] (#python-tuples)
- [How PySynth Works] (#how-pysynth-works)

## Python: Classes

In this project you will be working with Python classes. Here's a sample Python class, ```Album```, and some notes about it:

```python
class Album(object):

    # Albums all have backup music
    def backupMusic(self):
        print 'lalala'

    # Some albums have deluxe versions
    def deluxe(self):
        self.deluxeVersionExists = True;
    
    # Some songs have co-writers who get a cut
    def payCoWriters(self):
        return 100000
```

* To declare a class, you write ```class ClassName(object)```, which is like saying "the class ClassName *is an* object". The "is an object" part gets into inheritance, which is discussed in the next section.

* To instantiate an ```Album``` object, you could do something like the following: 

```python
a = Album()
```

* There's no concept of public or private with Python classes. Everything is by default accessible to the public, which eliminates the needs for getters or setters. To access class member attributes, you use the dot operator, as in C++.

* The keyword ```self``` in Python basically means “this instance of the class”. You only use it inside the definition of the class. For every function in a class, ```self``` will be the first parameter, and class member variables always start with ```self```, followed by the dot operator. For example, ```self.deluxeVersionExists``` means that this instance of the ```Album``` class has a member variable called ```deluxeVersionExists```.

* While ```self``` gets passed as a parameter in the definitions of member functions, you don't need to use it when calling those functions. So assuming you instantiated an ```Album``` object called ```a```, you could do something like ```print a.payCoWriters()```. Note that the ```payCoWriters()``` function takes no arguments, even though ```self``` is passed in the definition.


## Python: Inheritance

Inheritance is used to simplify a group of related objects. With inheritance, there is a parent class and one or more child(ren) class(es). The parent class is a base from which the children classes inherit functions and data, and those children are exactly like the parent in that respect. However, while the children classes do everything the parent does, they can also have unique functions and data. To illustrate, here's an example of inheritance.

Suppose we wanted to represent The Beatles' albums with Python. We can start by making an Album class to represent the generic music album (code given in the section above). Let's designate this class as the parent class, since all albums have certain traits in common.

However, we also acknowledge that each album is unique in some respects. Therefore, the different Album objects that we make must be able to share some traits and functions, but not all. This is why inheritance is important. Here are two Python classes that inherit from the above-defined ```Album``` class; note the similarities and differences to their parent class.

```python
class WhiteAlbum(Album): # The White Album is an Album

    # "Blackbird" is a song from the White Album
    def blackbird(self):
        print 'Blackbird fly!'                

    # "Blackbird" uses actual bird calls in the background
    def backupMusic(self):
        print 'tweet tweet'

class AbbeyRoad(Album): # Abbey Road is an Album

    # "Here Comes The Sun" is a song from Abbey Road
    def hereComesTheSun(self):
        print 'Here comes the sun'             

    # The backup line from "Here Comes The Sun" 
    def backupMusic(self):
        print 'do do do do'
```


Here are the main computer science takeaways from this example:

* If you make an object of any of these classes, you can call ```backupMusic()```, ```payCoWriters()```, or ```deluxe()``` on it, even though the last two functions were not redefined in the ```WhiteAlbum``` and ```AbbeyRoad``` classes. This eliminates redundant code!

* Calling ```backupMusic()``` on objects of the ```Album``` children classes will print different things for different classes. This is because the ```backupMusic()``` function was redefined, or overridden, in the child classes. Therefore, you can have several objects and treat them identically in your code, and they will all know what to do.

* Child classes can have functions the parent doesn't. ```WhiteAlbum``` has the ```blackbird()``` function, while ```AbbeyRoad``` has the ```hereComesTheSun()``` function. An object of the ```WhiteAlbum``` class cannot call ```hereComesTheSun()``` and an object of the ```AbbeyRoad``` class cannot call ```blackbird()``` -- which makes sense from a real-world point of view, when you think about which songs belong to which albums. An object of the parent class ```Album``` cannot call either.


## Python: Tuples

In Python, tuples are **immutable** sequences of objects. Like lists, they can contain objects of different types, can be indexed using square brackets starting at 0, and can be sliced. However, they cannot be changed after creation. Also, they are written using parentheses instead of square brackets.

Here are some examples:

```python
beatlesTuple = ('all', 'you', 'need', 'is', 'love')
nestedTuple = (1, (2, (3, 4)))
multiTypeTuple = (3.14, 'pi')
```

Because tuples are immutable, you **cannot** do things like:

```python
multiTypeTuple = (3.14, 'pi')
multiTypeTuple[0] = 3.14159 # TypeError: 'tuple' object does not support item assignment
```

If you *really* wanted to update a tuple, you could make a new tuple out of existing tuples:

```python
song1 = ('hey', 'jude')
song2 = ('dont', 'make', 'it', 'bad')
song3 = song1 + song2 # song3 is now a tuple
```

## How PySynth Works

**[PySynth](https://mdoege.github.io/PySynth/)** is a music synthesizer for Python that takes music notes in the form of a list of tuples, and outputs those notes to a .wav file with a name of your choice.

In PySynth, a note is represented as a *(string, integer)* tuple. The first item in the tuple is the pitch of the note, which tells you the "high" or "low" the note will be. The second item in the tuple is the duration of the note, which tells you how long the note will last. 

You will need to parse and/or create PySynth tuples in the [```getNextNote```](#./4.-Reach#getnextnote) function. Therefore, let's take a look at some example PySynth tuples to get a feel for how they work:

```python
c = ('c4', 2)
fSharp = ('f#6', 1)
aFlat = ('ab3', 16)
rest = ('r', 4)
```

Let's break apart each of these tuples.

- The first one, ```c```, is a note with a pitch of "c4". The 4 in "c4" gives us information about the note's *octave* value: in other words, it represents how high this note is relative to other notes. For example, "c5" would be higher than "c4", and "c3" would be lower than "c4". This note has a duration of 2, which means this note is relatively longer than most other notes. (See the [PySynth documentation](https://mdoege.github.io/PySynth/) for more specifics about how a note's duration is represented - we won't go into details here, but if you are interested the documentation has plenty of information).
- The second tuple, ```fSharp```, is a note with a pitch of "f#6". The "#" in "f#6" is a musical symbol which means that this pitch is higher in pitch than a regular "f". Note that this means that "f#" and "f" are different notes! Its duration is 1, which means that this note is the longest note that PySynth can play.
- The third tuple, ```aFlat```, is a note with a pitch of "ab3". The "b" in "ab3" is a "flat", which is a musical symbol that means that this pitch is lower in pitch than a regular "a". Its duration is 16, which means that this note is relatively shorter than most other notes.
- The final tuple, ```rest```, is a musical "pause" - the 'r' means a silence, and the 4 specifies how long the silence will last.

To actually generate a .wav file using PySynth, you call ```pysynth.make_wav(tuplesList, fn=songName)```. The first parameter, ```tuplesList```, is a list of PySynth tuples that represents a song. The second parameter tells PySynth what to name the output .wav file. Here is an example program that uses PySynth to output a song named test.wav:

```python
import pysynth
mySong = [ ('e4', 4), ('d4', 4), ('c4', 4) ]
pysynth.make_wav(mySong, fn='test.wav')
```

Try running this in the Python interpreter from the root Creative AI directory and watch what happens!