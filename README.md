# [GDL-playground](https://github.com/runxel/GDL-playground)
Playground for my GDL objects. Acts like pile of snippets. So if you see something of interest: Go and grab it and use it in your own projects.

_Your mileage may vary._

[![No Maintenance Intended](http://unmaintained.tech/badge.svg)](http://unmaintained.tech/)

No support!
It's just for learning, folks.

---

**Check out [my production objects](https://github.com/runxel/ArchiCAD-Objects) and find out how you can [boost your productivity](https://github.com/runxel/GDL-sublime) while programming your own GDL objects for Archicad!**

---

## [Get User Language](objects/Get-Language)
![Compatibility](https://img.shields.io/badge/compatibility-tbd-lightgrey?style=flat-square&logo=archicad&logoColor=white)
![Dependencies](https://img.shields.io/badge/dependencies-none-a9dfbf?style=flat-square)

See [full article](https://lucasbecker.de/posts/detecting-user-s-language-via-gdl) on how to detect (kinda) the language the user might speak.


## [Image Filter](objects/Image-Filter)
![Compatibility](https://img.shields.io/badge/compatibility-18_▲-lightgrey?style=flat-square&logo=archicad&logoColor=white)
![Dependencies](https://img.shields.io/badge/dependencies-none-a9dfbf?style=flat-square)

DEMO for a image filter process. The filter (via a prefix) serves to limit the otherwise very large and performance-heavy image selection in a GDL object.


## [Rounded Prisma](objects/Rounded-Prisma)
![Compatibility](https://img.shields.io/badge/compatibility-21_▲-lightgrey?style=flat-square&logo=archicad&logoColor=white)
![Dependencies](https://img.shields.io/badge/dependencies-yes-ff7979?style=flat-square)

This object is a demo for a modeling case where you have two vectors with their angles and need a rounded corner where those vectors meet. How to get the coordinates?
The solution is to offset the vectors by the radius and offset their intersection point back onto the original vectors.

Note: uses the ["BasicGeometricCalc" macro](http://gdl.graphisoft.com/tips-and-tricks/calling-basicgeometriccalc-macro), which is now deprecated. Could easily be rewritten to use the newer, dict-based ["BasicGeometry" macro](http://gdl.graphisoft.com/tips-and-tricks/using-basicgeometry-macro).

![Rounded Prisma](img/rounded-prisma.png)

---

### Basic Stuff

- To automagically get a proper font selection make a new string parameter and name it `fontType`.  
But what if you need to specify multiple font faces? Also working automatically are `fontTypeHeader`, `fontTypeText`, `fontTypeWatermark`, `strFontTypeCustOrig`and `strFontTypeID` – and possibly more, used by GS-scripted objects.
If that's still not enough do the following in the parameter script (with `myFontFace` being a string parameter):
```
! param script !
DIM fontNames[]
n = REQUEST("FONTNAMES_LIST", "", fontNames)
VALUES "myFontFace" fontNames, CUSTOM
```
- You can't pass parameters or arguments to subroutines. Instead you have to fall back to something I call "nasty caller":  
```
if x = y then
	nasty_caller = 1
else
	nasty_caller = 2
endif
gosub "mysubroutine"
END !----------------------------!

"mysubroutine":
	i = foo * nasty_caller
	! (...)
return
```
- [How to put an INFIELD over an UI_PICT in User Interface?](https://archicad-talk.graphisoft.com/viewtopic.php?f=6&t=69617):  
You have to write the `UI_INFIELD` twice into your UI script: once before and once after the `UI_PICT` command.

---

_Tautological boilerplate: All trademarks and copyrights on this page are the property of their respective owners._