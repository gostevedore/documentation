---
title: Builder
weight: 20
---

{{< hint warning >}}
TODO
{{< /hint >}}

Builder definition must match to golang struct defined below. Options structure depends on each driver.
{{< highlight golang "linenos=table" >}}
    type Builder struct {
        Name    string                 #u0060yaml:"name"#u0060
        Driver  string                 #u0060yaml:"driver"#u0060
        Options map[string]interface{} #u0060yaml:"options"#u0060
    }
{{< /highlight>}}

Set of builders must match to golang struct defined below
{{< highlight golang "linenos=table" >}}
    type Builders struct {
        Builders map[string]*Builder #u0060yaml:"builders"#u0060
    }
{{< /highlight>}}