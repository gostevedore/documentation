---
title: Image
date: "2020-01-31"
weight: 20
---

{{< hint warning >}}
TODO
{{< /hint >}}

Parser will use the SemVer struct to generate the template, and all SemVer attributes could be used to define each template.
{{< highlight golang "linenos=table" >}}
   // SemVer is a sematinc version representation
   type SemVer struct {
   	Major      string
   	Minor      string
   	Patch      string
   	PreRelease string
   	Build      string
   }
{{< /highlight>}}