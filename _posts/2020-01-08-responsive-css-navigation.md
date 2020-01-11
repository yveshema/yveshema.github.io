---
author: Shema
title: How to build responsive navigation in pure CSS
categories: [CSS, responsive design]
tags: [css,html,responsive,web]
---

I have been working on my personal github pages website and I wanted it to be responsive. One of the things to consider in responsive design is navigation. In many cases the navigation bar we are accustomed to interacting with on the desktop is not suitable for smaller devices. In the absence of a responsive approach, a number of usability issues could arise, including the navigation bar overlapping other content or being squashed to much to be of any use.

This is such a common problem in web development that there is no shortage of off-the-shelf solutions out there for it. Many <span class="CSS">CSS</span> frameworks, including the excellent [Materialize <span class="CSS">CSS</span>](https://materializecss.com/ "reference"), offer elegants solutions to this problem.  So you might be wondering why I would insist on reinventing the wheel. There could be a number of reasons why you would want to roll out your own widgets, but I will submit only three main ones which apply in my situation.

First, this kind of exercise is great for learning. Frameworks can be extremely valuable tools and are great for productivity, but in my humble opinion they are not helpful for learning. Think about it for a brief moment. The authors of these frameworks are no doubt very smart, but they also invested time and energy to understand the problems they were trying to solve in order to design the perfect solutions. We think of these fine folks as experts, yet expertise comes from learning and this is, in my view, within everyone's reach.

Second, many of these frameworks rely on <span class="Javascript">Javascript</span> to produce the intended effects. This is absolutely fine. However, some users might have reasons to disable <span class="Javascript">Javascript</span> while browsing the web. For situations like these, a solution based on pure <span class="CSS">CSS</span> would fare much better. 

Third, if all you want to do is place a single widget on a page, requiring an entire <span class="CSS">CSS</span> framework can feel like overkill. It is not uncommon for some frameworks to declare a dependency on Jquery for example in addition to their own helper <span class="Javascript">Javascript</span> modules. For a small site this could mean a significant performance hit. Sometimes simple is better than complex. A few line of <span class="CSS">CSS</span> code - as I will demonstrate shortly - might be all you need to solve the problem.

### <span class="CSS">CSS</span> <code>display</code> property

The technique I outline in this post is nothing more than basically controlling the visibility and positioning of block elements. You can show and hide <span class="HTML">HTML</span> elements using <span class="CSS">CSS</span> <code>display</code> property which is how the menu widget's drop-down effect is achieved. But before I delve into that, allow me to first describe the effect I am aiming at. I create two navigation components, one for each use case, that is, one to be shown on larger screen sizes and the other on smaller devices. This should work in a responsive manner. The navigation for smaller devices should be a dropdown hamburger menu widget that has become ubiquitous with the rise of mobile web. You can see this effect for yourself by changing the width of the browser as you read this post.

Below is the <span class="HTML">HTML</span>: 
{% gist cbdf2e85bf9e8364afb90b15e672bef9 index.html %}

As you can see it's a simple <span class="HTML">HTML</span> structure.

Next the <span class="CSS">CSS</span>:
{% gist 9fb486d18398716021e662ab1e8ccdda style.css %}

The <code>menu</code> <span class="CSS">CSS</span> class represents our menu widget. As a visual cue, I use an icon from the [Material Icons](https://materializecss.com/icons.html "reference") set. The menu items themselves are within the inner <code>ul</code> element which is initially hidden by setting its <code>display</code> property to <code>none</code>.

Most drop-down menus are triggered by a hover event. But for this use case, I found that to be of little use because the effect lasts only as long as the cursor remains above the element being hovered, which often makes selecting menu items a challenge. The same goes for the active state. So I found the focus event to be the suitable candidate for triggering the drop-down. This allows the user to dismiss it by clicking or tapping on a difference area of the page. 

So, all that needs to be done is to toggle the <code>display</code> property of the inner <code>ul</code> element when the outer <code>li</code> receives focus using the *:focus* pseudo class. Except that I couldn't figure out how to capture this event on a list item! A quick look-up on the [Mozilla Developer's Guide](https://developer.mozilla.org/en-US/docs/Web/API/<span class="HTML">HTML</span>OrForeignElement/focus "reference") reveals that <quote>[t]he focused element is the element which will receive keyboard and similar event by default</quote>. It turns out this generally means elements such as text inputs, links and buttons. Not to worry though, as it also turns out another pseudo class, *:focus-within* enables us to achieve exactly what we want. As its name suggests, it allows to know when an element inside another element has received focus so we can act on it. 

### <span class="CSS">CSS</span> positioning

The technique described above works, but still doesn't look idiomatic enough. A standard hamburger menu often occupies a fair amount of the device's real estate if not the entire viewport for accessibility reasons. To do that, I needed to turn to <span class="CSS">CSS</span> positioning. I used fixed positioning to snap the menu at the top left edge of the screen and gave it 60% width and 100% height. The end result looks more like a native menu widget. 

Below is the <span class="CSS">CSS</span> from the final iteration:
{% gist cbdf2e85bf9e8364afb90b15e672bef9 style.css %}

### Final thoughts

I hope I was able to make the case that with only a handful of <span class="CSS">CSS</span> code and a simple <span class="HTML">HTML</span> structure it is possible to build web widgets that look and feel like any you would get from sophisticated frameworks. To be clear, in no way is this a repudiation of the importance of third-party tools to web development. Nor am I presenting this as the most elegant solution to the stated problem. That being said, reinventing the wheel can be a great way of learning things and might even lead to some measure of performance boost for your application. To me that sounds like an excellent motivation to continue on this path. 