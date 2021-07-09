---
title: Talk graphics with Adobe After Effects
date: 2021-07-21
draft: true
---

I recently started experimenting with some of the Adobe products that most but perhaps not all institutions (at least in the US) have a subscription to.

In particular, I've been working with Adobe After Effects. It's by no means a <a href="https://en.wikipedia.org/wiki/Adobe_After_Effects">new program</a>, but it escaped my radar for a long time. This is probably because I only recently switched away from desktop linux to MacOS, and in Fall 2019 I started learning how to use Adobe Premiere Pro to edit my videotaped class lectures. Traditionally, I've turned to tools like Python/matplotlib, D3.js, or just simple Powerpoint/Keynote graphics to get the job done. These work great, and there are definitely reasons why these are mainstays of the data analysis workflow (at least the ones I'm familiar with in astronomy/astrophysics).

(unfortunately, as far as I can tell, Adobe After Effects is not available for Linux).

I dove into D3.js for webplotting, and the ability to compose beautiful graphics is stunning https://d3js.org/

Nowadays, though, I find that my medium for communications are either journal figures (for which I'll use matplotlib) or lectures presented through Keynote (or equivalently PowerPoint or Google Slides, etc.). And increasingly, I'll want to use some sort of animation to better convey the concept. I found that D3.js very powerful for this type of interactive data visualization, and while I was finishing my Ph.D. thesis I spent a lot of time writing D3.js visualizations that were part of a reveal.js slideshow, presented through the browser. This can look stunning, but I found that most of the rich capabilities that were gained by this really weren't used in a (live) talk format, because I would be the one doing the interacting with the figure. Also, I'd be busy talking about the slide content anyway, and so what I really needed was just a "play" button. So, for this format, I think a simple animated movie can work a lot better.

Recently, I've really been excited about Matplotlib's improved <a href="https://matplotlib.org/3.3.3/api/animation_api.html">animation functionality</a>. Zach Berta-Thompson has a very helpful <a href="https://github.com/zkbt/how-to-make-movies">github repository</a> with examples of how to use this.

but, at least for talks, I'm usually the only one doing the interaction. Morevoer

Some helpful advice from a data visualization seminar taught by Alyssa Goodman when I was in graduate school---don't be afraid to take a plot and then import it into Adobe Illustrator and make it clearer. This makes a lot of sense when you're using other people's plots in your own visualizations. Because you are taking something in the historical record and adjusting it for your own display purposes, there is no risk of the data changing under your feet.

Part of the real challenge for this is that I want the ability to preserve all effects when the data is changed. So, for this reason, the make a graph and then tweak elements in illustrator doesn't make as much sense when I'm dealing with my own graphics.

And so the interactivity aspect of the datavisualization is actually not as important (at least for this primary application). Also, there is the real benefit of using a standard slide deck like

So until now, I've been using matplotlib's very useful animate functionality to make helpful figures.

Helpful websites: https://workbench.tv/tutorials/2019-07-12_CSVgraphs

https://www.infoaperture.com/

Then I found I wanted to do some sort of easing...,

Working with After Effects is much more like working with a WYSIWYG editor (vs. say, LaTeX). As such, there are many (infinite) ways to do something.

And so the construction process is much more a dialog between code, tweaking, and graphical manipulation. If what you're after is a push button experience inside a single programming language, then this isn't really for you. Instead, in the process of creating a graphic, you'll be in dialog between Python code, Adobe After Effects GUI, Adobe Expressions (JavaScript), and possibly some Adobe Scripts (also JavaScript). The difference between my workflow before is that I think now all of the tedious figure tweaking is now carried out inside of the Adobe After Effects GUI, which helps speed up the process a bit. The downside is now that there are several moving parts between different codes and spaces and it takes some coordination.

I think of it more like wiring up a bunch of circuits together rather than clicking go and building everything from scratch each time.

But I kept finding that with my matplotlib routines, I needed to tweak transitions endlessly to get the graphic just right, something that's much better carried out using Adobe After Effects.

&nbsp;

But, you'll need to oscillate between writing scripts and expressions, you can create a very nice animation. Keeping things organized and reproducible. This isn't quite the same as doing a single script, change some parameters, and you can broadcast for many different outputs. If you're doing multiple animations for slightly different datasets, this isn't the workflow for you. But, you can keep things organized in the Adobe After Effects document using groups, layers, and so forth. It's just that now the products in the After Effects document are integrated into a full ecosystem. Sometimes, it's much easier to create and tweak a rectangle or arrow in Adobe AE for example, than it is to create and tweak such a thing in matplotlib. That's the point here.

Right now I've settled on the following workflow:
<ul>
 	<li>Static figure for journal article publication or talk slide: Matplotlib</li>
 	<li>"Simple" animations: Matplotlib, or in a talk format, Matplotlib + Keynote animation tools</li>
 	<li>Web interactive visualizations: D3.js or Bokeh (I don't do much of these)</li>
 	<li>"Complex" animations: Matplotlib for source files (e.g., <code>imshow</code> calls)</li>
</ul>
After Effects Scripting Guide: https://ae-scripting.docsforadobe.dev/ Super helpful

&nbsp;

https://www.youtube.com/watch?v=Wkr_XOpsAFU&amp;ab_channel=Animoplex