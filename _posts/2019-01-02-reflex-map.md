---
layout: post
title:  "Converter of maps from Reflex Arena to QuakeWorld"
date:   2019-01-02 20:41:55 +0100
categories: lisp
tags: lisp
---
# Converter of maps from Reflex Arena to QuakeWorld
Implemented a [converter](https://github.com/fourier/reflex-map/) of the maps from the game [Reflex Arena](http://reflexarena.com/) to the format understood by [TrenchBroom](https://github.com/kduske/TrenchBroom) - the editor for the maps for Quake-family of the games.

The releases (for Windows) could be found [here](https://github.com/fourier/reflex-map/releases)

The idea was to port the most popular map [The Catalyst](http://thc333.com/reflex/#tenth) by **tehace** to **QuakeWorld** for duel style of the gameplay. I've got an "ok" from the original author of the The Catalyst map (**tehace**) on this work.

The complication was to convert the scale along with the geometry, particularly vertical scale should be reduced.

The work was done mainly in Linux/SBCL during evenings, "pretending" to sleep while my wife was trying to get our 3yo daughter to sleep(which is extremely hard at this age!). The Windows releases with GUI were made in LispWorks 7.0 HobbyistDV for Win32. 


2 specific libraries helped me with this work: [3d-matrices](https://github.com/Shinmera/3d-matrices) by Shinmera and [cl-yacc](https://www.irif.fr/~jch/software/cl-yacc/) by Juliusz Chroboczek.

I've surprised with a quality of CL-YACC, no problems at all. A little problem encountered with 3d-matrices as I was unable to compile it on LispWorks due to [this bug](https://github.com/Shinmera/3d-matrices/issues/9) which Shinmera has fixed in like minutes after I posted the issue! Kudos to him.

The parsing of the Reflex maps was interesting in a sense that it used indentation for the grouping, like Python. So the lexer has to do the counting of indents and insert **INDENT** and artificial **DEDENT** tokens into the output list of tokens.

The problem was also that the Reflex Map format uses not natural coordinates order, so the transformation x z y -> x y z has to be performed while parsing.

So the job was to parse the file and generate a new file from the parsed, applying transformation (scaling in z direction) along the way.

Interesting was to implement embedding "prefabs" - the pre-defined geometry structures - into the output global geomerty (brushes). The prefabs are always defined in own separate coordinates and "used" with a position vector and the angles. Fun part is that prefabs could embed another prefabs. So the solution was to recursively output prefabs, applying transformations along the way.

Also it was fun to implement the lexer function as [a single LOOP statement](https://github.com/fourier/reflex-map/blob/b0b22aa063f3171c94b2e7881af0a51eaad60c44/src/reflex-map.lisp#L181).

The approach to create such utilities: implement them in SBCL/Linux testing in REPL and then create a GUI separately in LispWorks looks promising. Probably need to create a good skeleton for this kind of projects to use with cl-project, so the tools with simple Windows UI could be set up in a minutes.

