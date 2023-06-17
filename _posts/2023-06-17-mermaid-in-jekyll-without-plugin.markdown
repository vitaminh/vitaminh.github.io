---
layout: post
title:  "Using Mermaid diagrams with Jekyll without a plugin"
date:   2023-06-17 12:00:00 -0700
---
{% include mermaid.html %}

[Mermaid][mermaidSite] is an open source charting and diagramming tool that I've wanted to play with for a while so I thought trying to get it to work in Jekyll would be a fun experiment. Mermaid can take code like this:

```
graph LR
  A[Square Rect] -- Link text --> B((Circle))
  A --> C(Round Rect)
  B --> D{Rhombus}
  C --> D
```

and turn it into a diagram:

<div class="mermaid">
graph LR
    A[Square Rect] -- Link text --> B((Circle))
    A --> C(Round Rect)
    B --> D{Rhombus}
    C --> D
</div>

or this:

```
pie
  "Dogs" : 386
  "Cats" : 85.9
  "Rats" : 15
```

into a chart:

<div class="mermaid">
	pie
		"Dogs" : 386
		"Cats" : 85.9
		"Rats" : 15
</div>

There are several Jekyll plugins out there that enable use of Mermaid but for those of you who like to take the hands-on approach, here's how I got it working without a plugin.

## Step 1: Create a new file: _includes/mermaid.html

This new `html` file should look like this:

```
<script type="module">
  import mermaid from 'https://cdn.jsdelivr.net/npm/mermaid@10.2.3/dist/mermaid.esm.min.mjs';
  mermaid.initialize({
    startOnLoad:true,
    theme: 'dark'
  })
</script>
```

This script downloads the Mermaid module and initializes it. I have the `theme` option set to `dark`, but that's only necessary if the Jekyll theme you're using has a dark background. Otherwise, feel free to ignore it.

## Step 2: Include this new file wherever you want to embed a diagram/chart

Load the module in the markdown of any page/post you want to use Mermaid by adding this line of code:

```
{% raw %}
{% include mermaid.html %}
{% endraw %}
```

It's possible to load the module by altering the Jekyll theme's templates, but I prefer this approach simply because I didn't want to mess with the theme's files and risk having to deal with it in the future if they get updated.

## Step 3: Add the Mermaid code

Add a `div` and assign it the `mermaid` class. Within this `div`, add the Mermaid code for your diagram (check out the [extensive documentation][mermaidDocs] for all the stuff you can do):

```
<div class="mermaid">
  flowchart TD
    A[Start] --> B{Is it?}
    B -- Yes --> C[OK]
    C --> D[Rethink]
    D --> B
    B -- No ----> E[End]
</div>
```

With any luck, you should be seeing this:

<div class="mermaid">
flowchart TD
    A[Start] --> B{Is it?}
    B -- Yes --> C[OK]
    C --> D[Rethink]
    D --> B
    B -- No ----> E[End]
</div>

And that should do it! With the addition of one small `html` file, we have unlocked the power of Mermaid for our own use.

[mermaidSite]: https://mermaid.js.org/
[mermaidDocs]: https://mermaid.js.org/intro/
