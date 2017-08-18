---
title: Protocols and Kernels
separator: \n---- ?\n
verticalSeparator: \n-- ?\n
theme: jupyter
highlightTheme : darcula
revealOptions:
    transition: 'slide'
    slideNumber: 'c/t'

---

# Protocols and Kernels


Paul Ivanov – Matthias Bussonnier

JupyterCon – August 25, 2017, NYC

Slides available at Whatever

----

<!-- .slide: class="jupyter-light" -->

# About Us


Blah blah, Matthias BIDS, 
<!-- .element: class="fragment" data-fragment-index="1" -->


Paul Bloomberg
<!-- .element: class="fragment" data-fragment-index="2" -->


-- 

-- 

----


# Part II 

--

 - Rich display protocol
 - Tab Completion

<!-- 
In this section I'm going to dive into 2 parts:
- The rich display protocol, 
- Completion

-->
-- 
## The display protocol

Part of the Jupyter Protocol which allow the kernel to send data  to:

  - Be displayed to the user now
  - Be stored in a document (e.g., notebook)

-- 

### On he wire

It is part of the IOPub channel (Broadcasted)


In particular for __Display Data__

 ```
 {
    'data' : dict,
    'metadata' : dict,
    'transient': dict,
 }
```

<!-- 

If we do not dive into details on how to associate a request to a reply, 
we can see three fields: data, metadata and transient. 

The data field is a mapping from mime type to the actual data for this mimetype. – we'll come back to that just after 

The metadata field contain a mapping from mimetype to metadata for the
associated mimetype. The metadata can be safely ignored, but could contain extra
informations like preferred width/height for an image, 

The transient dict is for information which do make sens while the kernel is
alive, but should not be persisted in a document. 

Let's focus on the `data` field
-->

-- 

## `data`

Mimetype keyed dictionary, values should be interpreted by the frontend.

- Kernel sends _all_ mimetypes.
- Notebook frontend stores _all_ values.
- Allows _a posteriori_ choices 
- handling of custom unknown mimetypes by extensions

<!--

It is important to understand that this information is broadcasted, the kernel
does not have any idea that the execution request comes from 

  - Classic notebook
  - Nteract
  - JupyterLab
  - Hydrogen

Even if the execution comes from one of these it might not be the only one
listening. Hence we do not make assumptions about the frontend and send all the
mimetyes. 

This allow the frontend to decide, and in particular to store all information
for a posteriori re rendering. 


So far we have not introduced any language specificity in our description. 
Our kernel, which is a dispatching process is responsible from creating these
mimebundle. The mechanism by which this is done may not be part of the
underlying language. 

-->

-- 

## In IPython

In IPython the display protocol can be implemented in 2 ways:
  - Using `_repr_*_` methods
  - By registering `formatters`

It is triggered in 2 case:
  - When calling `display` on an object
  - On the result of the last expression in a cell

-- 

# Demo (Disp)


<!-- Hopefully also with the mimetype selector

Outline the difference between

In [1]: "Hello World"
...   : 41+1
Out[1]: 42

In [1]: print("Hello World")
...   : print(41+1)
Hello World 
42


Show one example of object with a custom repr
 -> GeoJson ? With the location of JupyterCon

Show one example of Registered formatter:
 -> Request Json expansion of dist

Show that you can select which repr to see. 

Try to do the list of images with the fake oreill'y books

--> 

-- 
 - Display Mimebundle 
 - Disp
 - Object define a REPR
 - User define a repr
 - GeoJson example
 - Maybe a selector that show that all the displayd output are sent. 

-- 

## Pushing Completion

-> Microsoft LS
Completion
portion of document

Cross language
Cell magic completion

--

# Thanks!


Slides at: 
