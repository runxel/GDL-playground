# [GDL-playground](https://github.com/runxel/GDL-playground)
Playground for my GDL objects. Acts like pile of snippets/Demo objects. So if you see something of interest: Go and grab it, salvage what you need and use it in your own projects.

[![No Maintenance Intended](http://unmaintained.tech/badge.svg)](http://unmaintained.tech/)  
No support, no maintenance. . _Your mileage may vary._  
It's just for learning, folks.

---

**Check out [my production objects](https://github.com/runxel/ArchiCAD-Objects) and find out how you can [boost your productivity](https://github.com/runxel/GDL-sublime) while programming your own GDL objects for Archicad!**

---
## [Demo Objects](objects/)

### [Array Sort](objects/Array-Sort.gdl)
Sort the values of an array, so they appear in alphanumeric order.

### [Changing Case of String](objects/Change-Case-of-String.gdl)
Directly changing the case is not directly possible with GDL. This is partly due to the fact that _GDL is generally case-insensitive_.

Test this statement with the following code:
```vb
if "ABCDE" = "abcde" then print "Same"
!` will output "Same"
```

Why the demo object works:  
We loop our way through every character in the string and replace it manually with the chosen case version. For this we need to have two conversion strings to be set up first, with the corresponding characters at the same place.  


### [Check if Macro](objects/Macro-Check.gdl)
Determine if the object is run alone or CALLed from another object.


### [Decimal Notation Conversion](objects/Decimal-Notation-Conversion.gdl)
The `SPLIT` command expects float numbers inside of strings to be in the American notation, ergo with a "dot" instead of a comma (as done in the European decimal notation).


### [Encode Integer to Binary](objects/Encode-Binary.gdl)


### [Delete Item from an Array](objects/Array-Delete-Item.gdl)
Sadly, there is no native way to delete/pop items from an array. This makes it necessary to do a bit of juggling.


### [Dyn Hotspots](objects/Dyn%20Hotspots)
![Dependencies](https://img.shields.io/badge/dependencies-none-a9dfbf?style=flat-square)

Explains how dynamic hotspots (in an array) work.  
Basically, it's this:
```vb
!--- Param script ---! 
values "n_in_array"   1, 2, 3, 4, 5, 6, 7, 8, 9, custom, range [1,)

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


### [Local Coor](objects/Local-Coor)
A subroutine for visual debugging (when you're lost in 3D).


### [Projection with custom attributes](objects/Projection%20Custom%20Attributes/Projection%20Custom%20Attributes/scripts/2d.gdl)
Demonstrates the unique ability of `project2{4}`: Overriding the attributes and e.g. changing the contour to a dashed line type.


### [Rounded Prisma](objects/Rounded-Prisma)
![Compatibility](https://img.shields.io/badge/compatibility-21_▲-lightgrey?style=flat-square&logo=archicad&logoColor=white)
![Dependencies](https://img.shields.io/badge/dependencies-yes-ff7979?style=flat-square)

This object is a demo for a modeling case where you have two vectors with their angles and need a rounded corner where those vectors meet. How to get the _coordinates_?
The solution is to offset the vectors by the radius and offset their intersection point back onto the original vectors.

Note: uses the ["BasicGeometricCalc" macro](http://gdl.graphisoft.com/tips-and-tricks/calling-basicgeometriccalc-macro), which is now deprecated. Could easily be rewritten to use the newer, dict-based ["BasicGeometry" macro](http://gdl.graphisoft.com/tips-and-tricks/using-basicgeometry-macro).

![Rounded Prisma](img/rounded-prisma.png)


### [Sort Buffer](objects/Sort-Buffer.gdl)
Sort numeric values on the buffer and ignore duplicate values.  
We solve this problem by comparing two values and switch them and repeat this until `is_unsorted` is false.


### [Sort Buffer into Array Randomly](objects/Sort-Buffer-Into-Array-Randomly.gdl)
Sort the values from the buffer randomly into an array.


### [Unique Array](objects/Unique-Array.gdl)
![Dependencies](https://img.shields.io/badge/dependencies-none-a9dfbf?style=flat-square)

Demo code for a way to ensure there are no duplicate items inside an one-dimensional array.


### [Shadow Param](objects/Shadow%20Param/Shadow%20Param/scripts/1d.gdl)
Shows a way of identifying which value in an array has changed last by user input by "shadowing" the whole parameter and performing a task afterwards.


### [Stretch Substitution](objects/Stretch%20Subst%20Demo/scripts/2d.gdl)
Demonstrates how you are able to substitute the param that is shown in the tracker, so you can steer another parameter.


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
All used variables in GDL _must_ be initialized. (It still works if you don't most of the time, but Archicad will yell at you nonetheless)  
A good way to init your arrays is to use a `for` loop.  
This holds true especially for Strings. Basically GDL only knows two basic types, those are: Number, and String. Booleans are Numbers internally, too.  
Every new variable used is pre-initialized as a number. You will get type errors if you set a new string var inside a subroutine and try to use it in the main script later.

#### All Relevant Stories correctly set up
```vb
!' Param script '!
ac_bottomlevel = GLOB_HSTORY_ELEV + GLOB_ELEVATION
ac_toplevel = ac_bottomlevel + ZZYZX
parameters  ac_bottomlevel = ac_bottomlevel,
            ac_toplevel = ac_toplevel
```

#### Font selection
To automagically get a proper font selection make a new string parameter and name it `fontType`. (There are a lot of similar parameters, that get a font selection. They are defined with others inside the `ARCHICAD_LIBRARY_MASTER.gsm`; Location in German lib: LCF > 4. Macros > Main XX : Base Macros XX)  

But what if you need to specify multiple font faces in different places and want to have a drop-down selection list ready?  
Do the following in the parameter script (with `myFontFace` being a string parameter):
```vb
!' param script !
dim fontNames[]
rrr = request("Fontnames_List", "", fontNames)
values "myFontFace" fontNames, CUSTOM
```

Pretty sure you have observed, that this system-generated font list will include fonts starting with the "@" sign ("at"-sign). Those fonts are used in vertical CJK context and shouldn't really be applied. At least outside of Asia those fonts are useless. For more info see [here](https://devblogs.microsoft.com/oldnewthing/20120719-00/?p=7093).  
A solution is to just pass the list through a loop sorting out fonts with an "@" in their name:
```vb
!' still param script !
dim reducedFontNames[]
k = 0
for i = 1 to vardim1(fontNames)
	if not(strstr(fontNames[i], "@")) then
		k = k + 1
		reducedFontNames[k] = fontNames[i]
	endif
next i
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


#### Reno Status
To get the renovation status of the object use `APPLICATON_QUERY`:
```vb
n = application_query("OwnCustomParameters", "GetParameter(Renovation.RenovationStatus)", parValue)
```

`parValue` will hold the reno status as _localised_(!) string.  
[This script](objects/Read-Built-In-Properties.gdl) will display all the built-in properties as text in the 2D. [<sup>Source</sup>](https://archicad-talk.graphisoft.com/viewtopic.php?f=49&t=70906)

<details>
  <summary>All Built-in Properties</summary>
	
	The whole list:  
	
```
! built-in properties
"Builtin.Design_Option_Name"
"Builtin.Design_Option_ID"
"Builtin.Design_Option_Set_Name"
"Builtin.General_Area"
"Builtin.General_Height"
"Builtin.General_NetVolume"
"Builtin.General_Width"
"Builtin.General_ElevationToProjectZero"
"Builtin.General_ElevationToFirstReferenceLevel"
"Builtin.General_ElevationToSecondReferenceLevel"
"Builtin.General_ElevationToSeaLevel"
"Builtin.General_ElevationToStory"
"Builtin.General_HomeOffset"
"Builtin.General_TopOffset"
"Builtin.General_SurfaceArea"
"Builtin.General_3DLength"
"Builtin.General_Thickness"
"Builtin.General_ConditionalVolume"
"Builtin.General_GrossVolume"
"Builtin.General_InsulationSkinThickness"
"Builtin.General_3DPerimeter"
"Builtin.General_FloorPlanPerimeter"
"Builtin.General_Holes3DPerimeter"
"Builtin.General_FloorPlanHolesPerimeter"
"Builtin.General_SlantAngle"
"Builtin.General_TopElevationToFirstReferenceLevel"
"Builtin.General_TopElevationToHomeStory"
"Builtin.General_TopElevationToProjectZero"
"Builtin.General_TopElevationToSeaLevel"
"Builtin.General_TopElevationToSecondReferenceLevel"
"Builtin.General_BottomElevationToFirstReferenceLevel"
"Builtin.General_BottomElevationToHomeStory"
"Builtin.General_BottomElevationToProjectZero"
"Builtin.General_BottomElevationToSecondReferenceLevel"
"Builtin.General_BottomElevationToSeaLevel"
"Builtin.General_OpeningNumber"
"Builtin.General_CrossSectionHeightAtEndPerpendicular"
"Builtin.General_CrossSectionHeightAtBeginPerpendicular"
"Builtin.General_CrossSectionWidthAtEndPerpendicular"
"Builtin.General_CrossSectionWidthAtBeginPerpendicular"
"Builtin.General_CrossSectionHeightAtEndCut"
"Builtin.General_CrossSectionHeightAtBeginCut"
"Builtin.General_CrossSectionWidthAtEndCut"
"Builtin.General_CrossSectionWidthAtBeginCut"
"Builtin.General_CrossSectionAreaAtBeginCut"
"Builtin.General_CrossSectionAreaAtEndCut"
"Builtin.Component_NetProjectedArea"
"Builtin.Component_Thickness"
"Builtin.Component_CrossSectionHeight"
"Builtin.Component_CrossSectionWidth"
"Builtin.Component_CrossSectionArea"
"Builtin.Component_NetVolume"
"Builtin.Component_GrossVolume"
"Builtin.Component_GrossProjectedArea"
"Builtin.Component_ConditionalProjectedArea"
"Builtin.Component_ConditionalVolume"
```
</details>




### Advanced

#### Label Placing
Place a label there, where the user has actually clicked (having no marker):
```vb
if not(LABEL_HAS_POINTER) then
	add2  LABEL_POSITION [2][1]	+ LABEL_POSITION [3][1],
		  LABEL_POSITION [2][2]	+ LABEL_POSITION [3][2]
endif
```
[<sup>[source]</sup>](https://archicad-talk.graphisoft.com/viewtopic.php?f=6&t=69980)

---

## Changes
Random experiences with changes in GDL.

### AC 24
While Archicad 24 did not bring any new GDL commands or functions, the HSF compared to 23 still _slightly_ changed. Inside the `libpartdocs.xml` file the `copyright` node lost its additional it hold before: `<Copyright SectVersion="1" SectionFlags="0" SubIdent="0">` is now just `<Copyright>`.


---

See also [selfGDL](https://selfgdl.de)

---

_Tautological boilerplate: All trademarks and copyrights on this page are the property of their respective owners._
