# dll2ahk

One of AutoHotKey's many awesome features is that is can load and call functions stored in dynamically-linked libraries (DLLs).  But it's got a couple big problems:
* The DllCall function is _really_ tricky.  Like stupid tricky.  I know C++ (a language used to actually create DLLs) and still struggle with it sometimes.  So it's not the kind of thing a newbie can easily pick up.
* Even for those of us who (think we) know what we're doing, once you have the DllCall figured out for a given function, we don't want to do it ever again.  So this means we usually end up writing AHK functions to wrap the mysterious DllCalls and posting it on the forum for the average Joe User to enjoy.  But that process is a _huge pain_ if you want to use a really big DLL.

But I've got an idea that I think will help solve these problems.  Most libraries that compile to DLLs acome with "header" files.  These are basically code files with the extension ".h" or ".hpp", that languages like C++ need, so they know what the DLL can do at compile-time.  That's right - there are _plain text files_ actually showing us what the DllCall needs.  That's an over-simplification and might not be 100% accurage (I'm thinkin' 98ish) but you get the gist.

Now, AHK is great at parsing text files - so why not use AHK to do this "dll-wrapping" for us?!  I guess a better name for the project would have been "h2ahk" or "hpp2ahk", because it's not actually converting binary code from DLLs to AHK (which would be nuts), but basically it's just a really advanced string-parsing script.  Here's how I see this working:

// The header file would give us something like this:
extern "C" __declspec(dllexport) int add(int x, int y);

// And AHK would give us this:
add(x, y)
{
    return DllCall("myDLL.dll/add", "int", x, "int", y, "Cdecl int")
}

## BOOM!

Now the average Joe User can just call the function, and we get this down REALLY well, we'll have a whole bunch of functions like this, ready to rock.  Of course there are all kinds of gotchas involved in making this work, so we're probably a long way from _there_, but if users can get the function signature from a header file and convert it to AHK, it would still take most of the tough work out of DllCall.

## Game plan

* For the first release, I'd like it to be able to convert a single function signature to an AHK function.  If it can do that much well, it'll be a HUGE help in a bunch of my projects.
* For the next version, I'd like to tackle preprocessor macros.  Without diving too deep into C++ stuff, lets just say these are code that can really mess things up.
* From there I'd like it to be able to handle structs.  Structs are basically objects that store data but have no methods.  They're used a lot in DllCall, and making them work involves manual memory allocation - as in old-school, bit-level, hard core C-style mayhem.  These are even tougher to figure out (in AHK terms you're using a ton of VarSetCapacity, NumGet, and other stuff few in the community really know how to use).  We totally gotta hook up our AHK peeps with a better way!
* And we'll take it from there.