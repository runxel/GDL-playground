# Get Language

A user on [architalk](https://archicad-talk.graphisoft.com/viewtopic.php?f=6&t=68666&p=306187#p306187) wanted to know, if there is the possibility to get the current language Archicad is using.  
While there is `REQ("GDL_Version")`, which tells us the version number of the running Archicad (at least indirectly, since `GDL_Version` and the Archicad version number relate to each other), there is no dedicated command to get to know the language the user is using.

However I found a solution to this enquiry:  
This solution is based on the fact that the user most probably will have loaded the default Archicad library.
At the root of the provided library is a XML file called _ARCHICAD Library.version_

It looks more or less like this:
```xml
<productversion>
  <buildnumber>2800</buildnumber>
  <projectbuildnumber>4010</projectbuildnumber>
  <codename>v2019</codename>
  <versionstring>23.0.0</versionstring>
  <shortversionstring>23</shortversionstring>
  <platform>MAC32</platform>
  <gslanguage>GER</gslanguage>
  <gsprodtype>FULL</gsprodtype>
  <prefspostfix>v2019 GER 2800.4010</prefspostfix>
  <guid>{6108D34D-A54F-49CE-A83C-9B805F956D74}</guid>
  <supersedes-lib name="BIBLIOTHEKEN 13" guid="{09C93C26-1CA0-11DF-B83D-B3B556D89593}"/>
  <supersedes-lib name="BIBLIOTHEKEN 14" guid="{988B0D68-1BEC-11DF-BC14-569455D89593}"/>
  <supersedes-lib name="BIBLIOTHEKEN 15" guid="{B559A49C-53AC-11E0-892A-2609DFD72085}"/>
  <supersedes-lib name="BIBLIOTHEKEN 16" guid="{736BF948-34A6-460F-8BE2-80AF101776B5}"/>
  <supersedes-lib name="BIBLIOTHEKEN 17" guid="{2DDCC60A-4E34-4DCC-8A21-C099C89DC870}"/>
  <supersedes-lib name="BIBLIOTHEKEN 18" guid="{BECC7064-F02C-4F35-91D0-613E6BD8D5DA}"/>
  <supersedes-lib name="BIBLIOTHEKEN 19" guid="{882FD682-7057-4C18-A761-40544EB588AD}"/>
  <supersedes-lib name="BIBLIOTHEKEN 20" guid="{FF6B2803-AC38-48C2-ABD6-6D56DD453444}"/>
  <supersedes-lib name="BIBLIOTHEKEN 21" guid="{7A18EB31-3044-4E99-8F54-B6CB16B6CE88}"/>
  <supersedes-lib name="BIBLIOTHEKEN 22" guid="{C46B01B7-097F-4F18-9B3D-71413F211E37}"/>
  <collapse-branches/>
</productversion>
```

What's interesting here is the `gslanguage` part: since users normally have no access to libraries not shipped with their Archicad, we can utilize that to see what language the user expects.

GDL has the XML Addon which will help to traverse this XML.
(It took me about an hour to figure out how this is actually supposed to work... – the GDL warnings are useless as always.)

See also the [Reference Guide](http://gdl.graphisoft.com/reference-guide/gdl-xml-extension).

In the end it is rather simple. Basically just four SLOC.  
[>>> FILE <<<](1d.gdl) 
(Heavily annotated so you can figure out whats happening) <sup>[1]</sup>

The `nodevalue` will be the three (?) letter string, `GER` in this case. I am not sure if `gslanguage` refers the country version or the language. E.g. Germany, Switzerland, and Austria speak German – have they their own `gslanguage` codes? Please tell me, if you know, so I can update this doc here!  
It at least definitely doesn't adhere to [ISO 3166-1 alpha-3](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-3) – German would be `DEU`...


[1] All this stuff is needed, because GDL as BASIC based language can't really work well with structured markup like XML. If you would need more data, you would work with loops.  
An example of that can be found [here](http://gdl.graphisoft.com/tips-and-tricks/how-to-use-the-gdl-xml-add-on).


Happy coding! 👋🏻💻
