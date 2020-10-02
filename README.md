# [GDL-playground](https://github.com/runxel/GDL-playground)
Playground for my GDL objects. Acts like pile of snippets/Demo objects. So if you see something of interest: Go and grab it, salvage what you need and use it in your own projects.

[![No Maintenance Intended](http://unmaintained.tech/badge.svg)](http://unmaintained.tech/)  
No support, no maintenance. . _Your mileage may vary._  
It's just for learning, folks.

---

**Check out [my production objects](https://github.com/runxel/ArchiCAD-Objects) and find out how you can [boost your productivity](https://github.com/runxel/GDL-sublime) while programming your own GDL objects for Archicad!**

---
## Objects

### [Dyn Hotspots](objects/Dyn%20Hotspots)
![Dependencies](https://img.shields.io/badge/dependencies-none-a9dfbf?style=flat-square)

Explains how dynamic hotspots (in an array) work.  
Basically, it's this:
```vb
!--- Param script ---! 
values "n_in_array"   1, 2, 3, 4, 5, 6, 7, 8, 9, CUSTOM, RANGE [1,)

dim setval[]
for i=1 to n_in_array
	if i > vardim1(array_val) then
		setval[i] = 0
	else
		setval[i] = array_val[i]
	endif
next i

array_val = setval

parameters   array_val = array_val
```


### [Get User Language](objects/Get-Language)
![Compatibility](https://img.shields.io/badge/compatibility-tbd-lightgrey?style=flat-square&logo=archicad&logoColor=white)
![Dependencies](https://img.shields.io/badge/dependencies-none-a9dfbf?style=flat-square)

See [full article](https://lucasbecker.de/posts/detecting-user-s-language-via-gdl) on how to detect (kinda) the language the user might speak.


### [Image Filter](objects/Image-Filter)
![Compatibility](https://img.shields.io/badge/compatibility-18_▲-lightgrey?style=flat-square&logo=archicad&logoColor=white)
![Dependencies](https://img.shields.io/badge/dependencies-none-a9dfbf?style=flat-square)

DEMO for a image filter process. The filter (via a prefix) serves to limit the otherwise very large and performance-heavy image selection in a GDL object.


### [Rounded Prisma](objects/Rounded-Prisma)
![Compatibility](https://img.shields.io/badge/compatibility-21_▲-lightgrey?style=flat-square&logo=archicad&logoColor=white)
![Dependencies](https://img.shields.io/badge/dependencies-yes-ff7979?style=flat-square)

This object is a demo for a modeling case where you have two vectors with their angles and need a rounded corner where those vectors meet. How to get the _coordinates_?
The solution is to offset the vectors by the radius and offset their intersection point back onto the original vectors.

Note: uses the ["BasicGeometricCalc" macro](http://gdl.graphisoft.com/tips-and-tricks/calling-basicgeometriccalc-macro), which is now deprecated. Could easily be rewritten to use the newer, dict-based ["BasicGeometry" macro](http://gdl.graphisoft.com/tips-and-tricks/using-basicgeometry-macro).

![Rounded Prisma](img/rounded-prisma.png)


### [Unique Array](objects/Unique-Array/Unique-Array.gdl)
![Dependencies](https://img.shields.io/badge/dependencies-none-a9dfbf?style=flat-square)

Demo code for a way to ensure there are no duplicate items inside an one-dimensional array.


### [Wählscheibe | Rotary dial](objects/Wählscheibe)
![Compatibility](https://img.shields.io/badge/compatibility-23_▲-lightgrey?style=flat-square&logo=archicad&logoColor=white)
![Dependencies](https://img.shields.io/badge/dependencies-none-a9dfbf?style=flat-square)

If you ever were in need to dial from Archicad. Just a Demo object to explore some concepts, e.g. using a hotspot based interface to let the user input things when in plan.  
_Note: Object doesn't actually call anybody, sadly._ 😢


---

## Further references
I dump some GDL insights here. 

### Basic Stuff

#### LP_XML Converter
Get yourself familiar with the LP_XMLConverter (get's shipped with Archicad). The most interesting option is to invoke it to convert to the HSF format:
```shell
LP_XMLConverter libpart2hsf <source> <dest>
```

#### Always init your vars
All used variables in GDL _must_ be initialised. (It still does work if you don't, but Archicad will yell at you nonetheless)  
A good way to init your arrays is to use a `for` loop.

#### Font selection
To automagically get a proper font selection make a new string parameter and name it `fontType`.  
But what if you need to specify multiple font faces? Also working automatically are `fontTypeHeader`, `fontTypeText`, `fontTypeWatermark`, `strFontTypeCustOrig`and `strFontTypeID` – and possibly more, used by GS-scripted objects.  
If that's still not enough do the following in the parameter script (with `myFontFace` being a string parameter):
```vb
!' param script !
dim fontNames[]
n = request("FONTNAMES_LIST", "", fontNames)
values "myFontFace" fontNames, CUSTOM
```

#### Passing variables
You can't pass parameters or arguments to subroutines. Instead you have to fall back to something I call "nasty caller":  
```vb
if x = y then
	nasty_caller = 1
else
	nasty_caller = 2
endif
gosub "mysubroutine"
END !'----------------------------!

"mysubroutine":
	i = foo * nasty_caller
	! (...)
return
```

You can however pass parameters when you are calling a macro. And there is even a second way: Macros are running in the same GDL context as the object from which the macro got called. This means you have access to the stack and you can pass information around this way, too. But beware, since you operating on the _same_ stack and not a copy you could easily mess up!

#### UI_Infield over an image
[How to put an INFIELD over an UI_PICT in User Interface?](https://archicad-talk.graphisoft.com/viewtopic.php?f=6&t=69617):  
You have to write the `UI_INFIELD` twice into your UI script: once before and once after the `UI_PICT` command.

#### Precision
Due to the nature of floating point math comparing 2 real numbers might not yield the result you think. `if real_a = real_b …` and therelike will result in the GDL editor yelling at you. To circumvent any errors you should rather subtract the two values and check if the result falls short of a specified [machine epsilon](https://en.wikipedia.org/wiki/Machine_epsilon), mostly abbreviated as `eps` in GDL code.
A very detailed structure could look like this:
```vb
dict EPS
EPS.length	= 0.0001				!' 1/10 mm
EPS.square	= EPS.length**2
EPS.scalar	= EPS.square
EPS.angle	= acs(1 - EPS.scalar)	!' 0.0081°
```
Determining if two numbers are the same would look like this now:
```
if (real_a - real_b) < EPS.length then [...] 
```

#### Deleting from an array
Sadly, there is no native way to delete/pop items from an array. This makes it necessary to do a bit of juggling:

```vb
dim arr[]
!' init w/ some values
arr[1] = 0.1
arr[2] = 0.2
arr[3] = 0.3

!' the item index you want to delete
todelete = 2

dim new_arr[]
check = 0
for i = 1 to vardim1(arr)-1
	if i = todelete then check = 1
	new_arr[i] = arr[i+check]
next i

arr = new_arr

print arr   !' prints:  0.1  0.3
```

#### Reno Status
To get the renovation status of the object use `APPLICATON_QUERY`:
```vb
n = APPLICATION_QUERY ("OwnCustomParameters", "GetParameter(Renovation.RenovationStatus)", parValue)
```

`parValue` will hold the reno status as _localised_(!) string.  
The following will display all the built-in parameters as text in the 2D: [<sup>Source</sup>](https://archicad-talk.graphisoft.com/viewtopic.php?f=49&t=70906)
```vb
DIM folderNamesArray[]
n = APPLICATION_QUERY ("OwnCustomParameters", "GetParameterFolderNames()", folderNamesArray)

for i = 1 to vardim1(folderNamesArray) step 3

    DIM parNamesArray[]
    querystring = "GetParameterNames(" + folderNamesArray[i] + ")"
    n = APPLICATION_QUERY ("OwnCustomParameters", querystring, parNamesArray)
    text2 0, 0, querystring
    add2 1, -1

    _nLines = 1
    for j = 1 to vardim1(parNamesArray) step 3

        parValue = ""
        querystring = "GetParameter(" + parNamesArray[j] + ")"
        n = APPLICATION_QUERY ("OwnCustomParameters", querystring, parValue)
        text2 0, 0, querystring + ": " + parValue
        add2 0, -1
        _nLines = _nLines + 1

    next j

    del _nLines
    add2 0, -_nLines

next i
```

### Advanced

#### Label Placing
Place a label there, where the user has actually clicked (having no marker):
```vba
if not(LABEL_HAS_POINTER) then
	add2  LABEL_POSITION [2][1]	+ LABEL_POSITION [3][1],
		  LABEL_POSITION [2][2]	+ LABEL_POSITION [3][2]
endif
```
<small>[[source](https://archicad-talk.graphisoft.com/viewtopic.php?f=6&t=69980)]</small>

---

## Changes
Random experiences with changes in GDL.

### AC 24
While Archicad 24 did not bring any new GDL commands or functions, the HSF compared to 23 still _slightly_ changed. Inside the `libpartdocs.xml` file the `copyright` node lost its additional it hold before: `<Copyright SectVersion="1" SectionFlags="0" SubIdent="0">` is now just `<Copyright>`.


---

See also [selfGDL](https://selfgdl.de)

---

_Tautological boilerplate: All trademarks and copyrights on this page are the property of their respective owners._