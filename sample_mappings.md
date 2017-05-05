
1. map all works under a single namespace to a single endpoint, retrieving: 
- text if it's a passage
- valid references if it's an edition
- metadata if it's anything else

```
/^(urn:cts:greekLit:(.*?):.+)$/              http://greekLit.sample.repository.org/cts/api?request=GetPassage&urn=$1
/^(urn:cts:greekLit:(.*?)\.(.*?)\.(.*+))$/   http://greekLit.sample.repository.org/cts/api?request=GetValidReff&urn=$1
/^(urn:cts:greekLit:(.*?))$/                 http://greekLit.sample.repository.org/cts/api?request=GetCapabilities&urn=$1
```

2. map a specific textgroup to a single endpoint, retrieving: 
- text if it's a passage
- valid references if it's an edition
- metadata if it's anything else

```
/^(urn:cts:greekLit:tlg0012\.(.*?):.+)$/       http://homer.sample.repository.org/cts/api?request=GetPassage&urn=$1
/^(urn:cts:greekLit:tlg0012\.(.*?)\.(.*+))$/   http://homer.sample.repository.org/cts/api?request=GetValidReff&urn=$1
/^(urn:cts:greekLit:tlg0012(\..*?)?)$/         http://homer.sample.repository.org/cts/api?request=GetCapabilities&urn=$1
```


3. map a specific work to a single endpoint, retrieving: 
- text if it's a passage
- valid references if it's an edition
- metadata if it's anything else

```
/^(urn:cts:greekLit:tlg0012\.tlg001\.(.*?):.+)$/   http://iliad.sample.repository.org/cts/api?request=GetPassage&urn=$1
/^(urn:cts:greekLit:tlg0012\.tlg001\.(.*+))$/      http://iliad.sample.repository.org/cts/api?request=GetValidReff&urn=$1
/^(urn:cts:greekLit:tlg0012\.tlg001(\..*?)?)$/     http://iliad.sample.repository.org/cts/api?request=GetCapabilities&urn=$1
```



4. map a specific edition to a single endpoint, retrieving: 
- text if it's a passage
- valid references if it's an edition

```
/^(urn:cts:greekLit:tlg0012\.tlg001\.perseus-grc2:.+)$/   http://perseusiliad.sample.repository.org/cts/api?request=GetPassage&urn=$1
/^(urn:cts:greekLit:tlg0012\.tlg001\.perseus-grc2$/      http://perseusiliad.sample.repository.org/cts/api?request=GetValidReff&urn=$1
```

5. map a specific passage in a specific edition to a single endpoint retrieving passage

```
/^(urn:cts:greekLit:tlg0012\.tlg001\.perseus-grc2:1((\.?[^-]+)?(-1\.?[^-]+)?))$/   http://perseusiliadbook1.sample.repository.org/cts/api?request=GetPassage&urn=$1
```

