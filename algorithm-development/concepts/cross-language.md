# Cross language

Because algorithms are exchanged through Docker images they can be written in any language. This is an advantage as developers can use their preferred language for the problem they need to solve.&#x20;

{% hint style="warning" %}
The [wrappers ](wrappers.md)are only available for R and Python, so when you use different language you need to handle the IO yourself. Consult the [Input & Output](input-and-output.md) section on what the node supplies to your algorithm container.
{% endhint %}

When data is exchanged between the user and the algorithm they both need to be able to read the data. When the algorithm uses a language specific serialization (e.g. a `pickle` in the case of Python or `RData` in the case of R) the user needs to use the same language to read the results. A better solution would be to use a type of serialization that is not specific to a language. For our wrappers we use JSON for this purpose.&#x20;

{% hint style="success" %}
Communication between algorithm containers can use language specific serialization as long as the different parts of the algorithm use the same language. &#x20;
{% endhint %}

