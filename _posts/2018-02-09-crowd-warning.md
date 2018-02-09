---
title: Crowd danger estimation and warning
layout: default
---
Any time an event or attraction draws a crowd, individuals may continue to move into the most crowded areas and inadvertently obstruct egress, creating dangerously dense-packed locations vulnerable to stampedes. In addition to the motivation of avoiding danger, people may also want to avoid highly congested spots because the experience is unpleasant. Distributed sensing and computing can help solve this problem.

Using Protelis and the principles of aggregate programming, it is straightforward to write a scalable and robust distributed application to detect severe crowding and help individuals navigate around highly congested areas. We illustrate the use of several core Protelis operators and built-in functions.
<b>Code samples and illustrations from a simulation:</b>
Using some of Protelis’s special operators (purple), a small number of functions (blue) can be defined and applied to variables (green), a crowd safety service can use information about the number of nearby devices to issue warnings about areas with dangerous crowding levels. 

The function dangerousDensity flags whether a location has a high or low danger. The function determines danger by estimating the local density of people, using the values of <i>p</i> (proportion of people with a participating device running this app), <i>r</i> (the radius of interest), and comparing to a density danger cut-off value of 2.17 people per square meter.

The function crowdTracking checks for a dangerous density in crowded areas, where 1.08 people per square meter is used as the cut-off to define crowded.

The function crowdWarning alerts individuals who are near dangerously crowded spots.

![map with green, yellow, and red dots](/images/marathon.png)

In this simulation of the 2013 Vienna marathon, dots represent devices running the crowd safety service. Red dots are devices in locations with potentially dangerous crowd density and yellow dots are devices providing crowd density warnings.

![map with blue, yellow, and red dots](/images/crowd-management.jpg)

In this simulation of public events near the Boston waterfront, dots represent devices running the crowd safety service. Red dots are devices in locations with potentially dangerous crowd density and yellow dots are devices providing crowd density warnings.

end

<pre>
<code style="color:red">def</code><code style="color:blue"> distanceTo</code><code>(</code><code style="color:green">source</code><code>) {</code>
<code style="color:red">  rep</code><code>(</code><code style="color:green">d</code><code> <- Infinity) {</code>
<code style="color:purple">    mux</code><code> (</code><code style="color:green">source</code><code>) { 0 }</code>
<code style="color:purple">    else</code><code> { minHood(</code><code style="color:purple">nbr</code><code>(</code><code style="color:green">d</code><code>) + nbrRange) }</code>
<code>  }
}</code>
</pre>


<pre>
<code style="color:red">def</code><code style="color:blue"> descend</code><code>(</code><code style="color:green">source</code><code>, </code><code style="color:green">potential</code><code>) {</code>
<code style="color:purple">  rep</code><code>(</code><code style="color:green">path</code><code> <- </code><code style="color:green">source</code><code>) {</code>
<code style="color:red">    let</code><code style="color:green"> nextStep</code><code> = minHood(</code><code style="color:purple">nbr</code><code>([</code><code style="color:green">potential</code><code>, </code><code style="color:purple">self</code><code style="color:blue">.getId</code><code>()]));</code>
<code style="color:purple">    if</code><code>(</code><code style="color:green">nextStep</code><code style="color:blue">.size</code><code>() > 1) {</code>
<code style="color:red">    let</code><code style="color:green"> candidates</code><code> = (</code><code style="color:purple">nbr</code><code>([</code><code style="color:green">nextStep</code><code style="color:blue">.get</code><code>(1), </code><code style="color:green">path</code><code>]));</code>
<code style="color:green">    source</code><code> || </code><code style="color:purple">anyHood</code><code>([</code><code style="color:purple">self</code><code style="color:blue">.getId</code><code>(), true] == </code><code style="color:green">candidates</code><code>)</code>
<code>    } </code><code style="color:purple">else</code><code> {</code>
<code style="color:green">      source</code>
<code>    }
  }
}</code>
</pre>


<pre>
<code style="color:red">def</code><code style="color:blue"> rendezvous</code><code>(</code><code style="color:green">person1</code><code>, </code><code style="color:green">person2</code><code>) {</code>
<code style="color:blue">  descend</code><code> (</code><code style="color:green">person1</code><code> == </code><code style="color:green">owner</code><code>, </code><code style="color:blue">distanceTo</code><code>(</code><code style="color:green">person2</code><code>) == </code><code style="color:green">owner</code><code>))</code>
<code>}</code>
</pre>
