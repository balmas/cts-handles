
## 1. map all works under a single namespace to a single endpoint, retrieving:
   
    a. text if it's a passage
    
    b. valid references if it's an edition
    
    c. metadata if it's anything else

```
/^(urn:cts:greekLit:(.*?):.+)$/              http://greekLit.sample.repository.org/cts/api?request=GetPassage&urn=$1
/^(urn:cts:greekLit:(.*?)\.(.*?)\.(.*+))$/   http://greekLit.sample.repository.org/cts/api?request=GetValidReff&urn=$1
/^(urn:cts:greekLit:(.*?))$/                 http://greekLit.sample.repository.org/cts/api?request=GetCapabilities&urn=$1
```

Sample URNs

  a. [urn:cts:greekLit:tlg0085.tlg002.perseus-grc2:1-20](http://cts.perseids.org/api/cts?request=GetPassage&urn=urn:cts:greekLit:tlg0085.tlg002.perseus-grc2:1-20)

  b. [urn:cts:greekLit:tlg0012.tlg001.perseus-eng4](http://cts.perseids.org/api/cts?request=GetValidReff&urn=urn:cts:greekLit:tlg0012.tlg001.perseus-eng4)

  c. [urn:cts:greekLit:tlg0096](http://cts.perseids.org/api/cts?request=GetCapabilities&urn=urn:cts:greekLit:tlg0096)

## 2. map a specific textgroup to a single endpoint, retrieving:

    a. text if it's a passage

    b. valid references if it's an edition
    
    c. metadata if it's anything else

```
/^(urn:cts:greekLit:tlg0012\.(.*?):.+)$/       http://homer.sample.repository.org/cts/api?request=GetPassage&urn=$1
/^(urn:cts:greekLit:tlg0012\.(.*?)\.(.*+))$/   http://homer.sample.repository.org/cts/api?request=GetValidReff&urn=$1
/^(urn:cts:greekLit:tlg0012(\..*?)?)$/         http://homer.sample.repository.org/cts/api?request=GetCapabilities&urn=$1
```

  a. [urn:cts:greekLit:tlg0012.tlg002.perseus-grc2:1.1](http://cts.perseids.org/api/cts?request=GetPassage&urn=urn:cts:greekLit:tlg0012.tlg002.perseus-grc2:1.1)

  b. [urn:cts:greekLit:tlg0012.tlg001.perseus-eng4](http://cts.perseids.org/api/cts?request=GetValidReff&urn=urn:cts:greekLit:tlg0012.tlg001.perseus-eng4)

  c. [urn:cts:greekLit:tlg0012](http://cts.perseids.org/api/cts?request=GetCapabilities&urn=urn:cts:greekLit:tlg0012)

## 3. map a specific work to a single endpoint, retrieving: 

    a. text if it's a passage
    
    b. valid references if it's an edition
    
    c. metadata if it's anything else

```
/^(urn:cts:greekLit:tlg0012\.tlg001\.(.*?):.+)$/   http://iliad.sample.repository.org/cts/api?request=GetPassage&urn=$1
/^(urn:cts:greekLit:tlg0012\.tlg001\.(.*+))$/      http://iliad.sample.repository.org/cts/api?request=GetValidReff&urn=$1
/^(urn:cts:greekLit:tlg0012\.tlg001(\..*?)?)$/     http://iliad.sample.repository.org/cts/api?request=GetCapabilities&urn=$1
```

  a. [urn:cts:greekLit:tlg0012.tlg002:1.1](http://cts.perseids.org/api/cts?request=GetPassage&urn=urn:cts:greekLit:tlg0012.tlg002:1.1)

  b. [urn:cts:greekLit:tlg0012.tlg002.perseus-grc2](http://cts.perseids.org/api/cts?request=GetValidReff&urn=urn:cts:greekLit:tlg0012.tlg002.perseus-grc2)

  c. [urn:cts:greekLit:tlg0012](http://cts.perseids.org/api/cts?request=GetCapabilities&urn=urn:cts:greekLit:tlg0012)


## 4. map a specific edition to a single endpoint, retrieving: 
    
    a.  text if it's a passage
    
    b. valid references if it's an edition
  
  
```
/^(urn:cts:greekLit:tlg0012\.tlg001\.perseus-grc2:.+)$/   http://perseusiliad.sample.repository.org/cts/api?request=GetPassage&urn=$1
/^(urn:cts:greekLit:tlg0012\.tlg001\.perseus-grc2$/      http://perseusiliad.sample.repository.org/cts/api?request=GetValidReff&urn=$1
```

  a. [urn:cts:greekLit:tlg0012.tlg002.perseus-grc2:1.1](http://cts.perseids.org/api/cts?request=GetPassage&urn=urn:cts:greekLit:tlg0012.tlg002.perseus-grc2:1.1)

  b. [urn:cts:greekLit:tlg0012.tlg002.perseus-grc2](http://cts.perseids.org/api/cts?request=GetValidReff&urn=urn:cts:greekLit:tlg0012.tlg002.perseus-grc2)


## 5. map a specific passage in a specific edition to a single endpoint retrieving passage

```
/^(urn:cts:greekLit:tlg0012\.tlg001\.perseus-grc2:1((\.?[^-]+)?(-1\.?[^-]+)?))$/   http://perseusiliadbook1.sample.repository.org/cts/api?request=GetPassage&urn=$1
```

[urn:cts:greekLit:tlg0012.tlg002.perseus-grc2:1.1](http://cts.perseids.org/api/cts?request=GetPassage&urn=urn:cts:greekLit:tlg0012.tlg002.perseus-grc2:1.1)

