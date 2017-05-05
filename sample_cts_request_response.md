```
http://cts.perseids.org/api/cts/?request=GetPassage&urn=urn:cts:greekLit:tlg0012.tlg002.perseus-grc2:1.1-1.20

Goal: Retrieve a specific passage of text from a specific edition:
URN: urn:cts:greekLit:tlg0012.tlg002.perseus-grc2:1.1-1.5
CTS Request Type: GetPassage 
CTS Response: 

<GetPassage xmlns:tei="http://www.tei-c.org/ns/1.0" xmlns="http://chs.harvard.edu/xmlns/cts">
  <request>
    <requestName>GetPassage</requestName>
    <requestUrn>urn:cts:greekLit:tlg0012.tlg002.perseus-grc2:1.1-1.5</requestUrn>
  </request>
  <reply>
    <urn>urn:cts:greekLit:tlg0012.tlg002.perseus-grc2:1.1-1.5</urn>
    <passage>
      <TEI xmlns="http://www.tei-c.org/ns/1.0">
        <text>
          <body>
            <div type="edition" n="urn:cts:greekLit:tlg0012.tlg002.perseus-grc2" xml:lang="grc">
              <div n="1" type="textpart" subtype="book">
                <l n="1">ἄνδρα μοι ἔννεπε, μοῦσα, πολύτροπον, ὃς μάλα πολλὰ</l>
                <l n="2">πλάγχθη, ἐπεὶ Τροίης ἱερὸν πτολίεθρον ἔπερσεν·</l>
                <l n="3">πολλῶν δʼ ἀνθρώπων ἴδεν ἄστεα καὶ νόον ἔγνω,</l>
                <l n="4">πολλὰ δʼ ὅ γʼ ἐν πόντῳ πάθεν ἄλγεα ὃν κατὰ θυμόν,</l>
                <l n="5">ἀρνύμενος ἥν τε ψυχὴν καὶ νόστον ἑταίρων.</l>
              </div>
            </div>
          </body>
        </text>
      </TEI>
    </passage>
  </reply>
</GetPassage>
```

```
http://cts.perseids.org/api/cts/?request=GetValidReff&urn=urn:cts:greekLit:tlg0012.tlg002.perseus-grc2
Goal: Find the available top level passages in an online edition of a work
URN: urn:cts:greekLit:tlg0012.tlg002.perseus-grc2
CTS Request Type: GetValidReff
CTS Response:
<GetValidReff xmlns:tei="http://www.tei-c.org/ns/1.0" xmlns="http://chs.harvard.edu/xmlns/cts">
    <request>
        <requestName>GetValidReff</requestName>
        <requestUrn>urn:cts:greekLit:tlg0012.tlg002.perseus-grc2</requestUrn>
        <requestLevel>1</requestLevel>
    </request>
    <reply>
        <reff>
            <urn>urn:cts:greekLit:tlg0012.tlg002.perseus-grc2:1</urn>
            <urn>urn:cts:greekLit:tlg0012.tlg002.perseus-grc2:2</urn>
            <urn>urn:cts:greekLit:tlg0012.tlg002.perseus-grc2:3</urn>
            <urn>urn:cts:greekLit:tlg0012.tlg002.perseus-grc2:4</urn>
            <urn>urn:cts:greekLit:tlg0012.tlg002.perseus-grc2:5</urn>
            <urn>urn:cts:greekLit:tlg0012.tlg002.perseus-grc2:6</urn>
            <urn>urn:cts:greekLit:tlg0012.tlg002.perseus-grc2:7</urn>
            <urn>urn:cts:greekLit:tlg0012.tlg002.perseus-grc2:8</urn>
            <urn>urn:cts:greekLit:tlg0012.tlg002.perseus-grc2:9</urn>
            <urn>urn:cts:greekLit:tlg0012.tlg002.perseus-grc2:10</urn>
            <urn>urn:cts:greekLit:tlg0012.tlg002.perseus-grc2:11</urn>
            <urn>urn:cts:greekLit:tlg0012.tlg002.perseus-grc2:12</urn>
            <urn>urn:cts:greekLit:tlg0012.tlg002.perseus-grc2:13</urn>
            <urn>urn:cts:greekLit:tlg0012.tlg002.perseus-grc2:14</urn>
            <urn>urn:cts:greekLit:tlg0012.tlg002.perseus-grc2:15</urn>
            <urn>urn:cts:greekLit:tlg0012.tlg002.perseus-grc2:16</urn>
            <urn>urn:cts:greekLit:tlg0012.tlg002.perseus-grc2:17</urn>
            <urn>urn:cts:greekLit:tlg0012.tlg002.perseus-grc2:18</urn>
            <urn>urn:cts:greekLit:tlg0012.tlg002.perseus-grc2:19</urn>
            <urn>urn:cts:greekLit:tlg0012.tlg002.perseus-grc2:20</urn>
            <urn>urn:cts:greekLit:tlg0012.tlg002.perseus-grc2:21</urn>
            <urn>urn:cts:greekLit:tlg0012.tlg002.perseus-grc2:22</urn>
            <urn>urn:cts:greekLit:tlg0012.tlg002.perseus-grc2:23</urn>
            <urn>urn:cts:greekLit:tlg0012.tlg002.perseus-grc2:24</urn>
        </reff>
    </reply>
</GetValidReff>
```

```
http://cts.perseids.org/api/cts/?request=GetCapabilities&urn=urn:cts:greekLit:tlg0012.tlg002.perseus-grc2
Goal: Retrieve Metadata and Repository holdings for an Edition

URN: urn:cts:greekLit:tlg0012.tlg002.perseus-grc2
CTS Request Type: GetCapabilities  
CTS Response: 

<GetCapabilities xmlns="http://chs.harvard.edu/xmlns/cts">
  <request>
    <requestName>GetInventory</requestName>
    <requestFilters>urn=urn:cts:greekLit:tlg0012.tlg002.perseus-grc2</requestFilters>
  </request>
  <reply>
    <ti:TextInventory xmlns:ti='http://chs.harvard.edu/xmlns/cts'>
      <ti:textgroup urn='urn:cts:greekLit:tlg0012' xmlns:ti='http://chs.harvard.edu/xmlns/cts'>
        <ti:groupname xml:lang='eng'>Homer</ti:groupname>
          <ti:work xml:lang="grc" urn='urn:cts:greekLit:tlg0012.tlg002' groupUrn='urn:cts:greekLit:tlg0012' 
            xmlns:ti='http://chs.harvard.edu/xmlns/cts'>
            <ti:title xml:lang='eng'>Odyssey</ti:title>
            <ti:edition urn='urn:cts:greekLit:tlg0012.tlg002.perseus-grc2' 
              workUrn='urn:cts:greekLit:tlg0012.tlg002' xmlns:ti='http://chs.harvard.edu/xmlns/cts'>
              <ti:label xml:lang='eng'>Odyssey, Loeb classical library</ti:label>
              <ti:description xml:lang='eng'>Homer, creator; Murray, A. T. (Augustus Taber), 1866-1940, editor</ti:description>
              <ti:online><
                ti:citationMapping>
                  <ti:citation label="book" xpath="/tei:div[@n='?']" scope="/tei:TEI/tei:text/tei:body/tei:div">
                    <ti:citation label="line" xpath="//tei:l[@n='?']" scope="/tei:TEI/tei:text/tei:body/tei:div/tei:div[@n='?']"></ti:citation>
                  </ti:citation>
                </ti:citationMapping>
              </ti:online>
            </ti:edition>
          </ti:work>
        </ti:textgroup>
      </ti:TextInventory>
  </reply>
</GetCapabilities>
```

```
http://cts.perseids.org/api/cts/?request=GetCapabilities&urn=urn:cts:greekLit:tlg0012.tlg002

Goal: Find out which editions of a work a repository has and rerieve metadata about them.
URN: urn:cts:greekLit:tlg0012.tlg002
CTS Request Type: GetCapabilities
CTS Response: 

<?xml version="1.0" encoding="UTF-8"?>
<GetCapabilities xmlns="http://chs.harvard.edu/xmlns/cts">
    <request>
        <requestName>GetInventory</requestName>
        <requestFilters>urn=urn:cts:greekLit:tlg0012.tlg002</requestFilters>
    </request>
    <reply>
        <ti:TextInventory xmlns:ti="http://chs.harvard.edu/xmlns/cts">
            <ti:textgroup urn="urn:cts:greekLit:tlg0012" xmlns:ti="http://chs.harvard.edu/xmlns/cts">
                <ti:groupname xml:lang="eng">Homer</ti:groupname>
                <ti:work xml:lang="grc" urn="urn:cts:greekLit:tlg0012.tlg002"
                    groupUrn="urn:cts:greekLit:tlg0012" xmlns:ti="http://chs.harvard.edu/xmlns/cts">
                    <ti:title xml:lang="eng">Odyssey</ti:title>
                    <ti:translation xml:lang="eng"
                        urn="urn:cts:greekLit:tlg0012.tlg002.perseus-eng3"
                        workUrn="urn:cts:greekLit:tlg0012.tlg002"
                        xmlns:ti="http://chs.harvard.edu/xmlns/cts">
                        <ti:label xml:lang="eng">Odyssey, Loeb classical library</ti:label>
                        <ti:description xml:lang="eng">Homer, creator; Murray, A. T. (Augustus
                            Taber), 1866-1940, translator</ti:description>
                        <ti:online>
                            <ti:citationMapping>
                                <ti:citation label="book" xpath="/tei:div[@n='?']"
                                    scope="/tei:TEI/tei:text/tei:body/tei:div">
                                    <ti:citation label="card" xpath="/tei:div[@n='?']"
                                        scope="/tei:TEI/tei:text/tei:body/tei:div/tei:div[@n='?']"/>
                                </ti:citation>
                            </ti:citationMapping>
                        </ti:online>
                    </ti:translation>
                    <ti:translation xml:lang="eng"
                        urn="urn:cts:greekLit:tlg0012.tlg002.perseus-eng4"
                        workUrn="urn:cts:greekLit:tlg0012.tlg002"
                        xmlns:ti="http://chs.harvard.edu/xmlns/cts">
                        <ti:label xml:lang="eng">Odyssey, The Odyssey of Homer rendered into English
                            prose for the use of those who cannot read the original Revised by
                            Timothy Power and Gregory Nagy</ti:label>
                        <ti:description xml:lang="eng">Homer, creator; Butler, Samuel, 1835-1902,
                            translator</ti:description>
                        <ti:online>
                            <ti:citationMapping>
                                <ti:citation label="book" xpath="/tei:div[@n='?']"
                                    scope="/tei:TEI/tei:text/tei:body/tei:div">
                                    <ti:citation label="card" xpath="/tei:div[@n='?']"
                                        scope="/tei:TEI/tei:text/tei:body/tei:div/tei:div[@n='?']"/>
                                </ti:citation>
                            </ti:citationMapping>
                        </ti:online>
                    </ti:translation>
                    <ti:edition urn="urn:cts:greekLit:tlg0012.tlg002.perseus-grc2"
                        workUrn="urn:cts:greekLit:tlg0012.tlg002"
                        xmlns:ti="http://chs.harvard.edu/xmlns/cts">
                        <ti:label xml:lang="eng">Odyssey, Loeb classical library</ti:label>
                        <ti:description xml:lang="eng">Homer, creator; Murray, A. T. (Augustus
                            Taber), 1866-1940, editor</ti:description>
                        <ti:online>
                            <ti:citationMapping>
                                <ti:citation label="book" xpath="/tei:div[@n='?']"
                                    scope="/tei:TEI/tei:text/tei:body/tei:div">
                                    <ti:citation label="line" xpath="//tei:l[@n='?']"
                                        scope="/tei:TEI/tei:text/tei:body/tei:div/tei:div[@n='?']"/>
                                </ti:citation>
                            </ti:citationMapping>
                        </ti:online>
                    </ti:edition>
                </ti:work>
            </ti:textgroup>
        </ti:TextInventory>
    </reply>
</GetCapabilities>
```


