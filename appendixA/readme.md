Appendix A : Data and reproducibility
=====================================

This folder contains code used to count words, and a draft of Appendix A, but this readme file is also a good place to start thinking about the gritty details of reproducing workflows in *Distant Horizons.*

Goals
-----

My goals in this repository are

1. To let people without programming experience simply inspect the list of texts used in a particular chapter, 
2. To make it possible for someone with moderate programming experience to actually reproduce calculations and figures, working from original data in the form of word counts.
3. To provide code that someone super-scrupulous could use to reproduce the word counting.

Limits
------

There are also some things I can't guarantee. 

1. The original texts are often in copyright, or covered by other IP contracts, so I can't share readable texts.
2. I don't intend to provide "tools," designed to be useful in other research projects. This is just code that I actually used in the process of answering a particular set of questions.
3. I also, frankly, can't even guarantee that everything is going to run in a simple push-button way. I've tried to make paths relative to the repo, but I may not have always succeeded; you might have to edit some things before they run.
4. Where predictive modeling is concerned, I can't guarantee that it will run *quickly.* Some of the modeling code took me an afternoon to run on a 12-core desktop. If you're running those elements of the workflow on a laptop, it could take a day, and you'll have a warm laptop.

Error
-----
The data and metadata in this repository contain many errors. When you're studying patterns across thousands of volumes, it is not possible to correct every OCR error. When you're studying hundreds of thousands of volumes, it's not even possible to guarantee that dates of publication are 100% correct. Library metadata is simply not 100% correct.

So the approach I have taken in this project is not to eliminate error. Instead I usually measure the level of error, assess how it varies across the timeline, and provide some indication of how much that variation would change my results. In the repositories for chapters 1 and 4 these calculations are gathered in a subfolder called **error.** 

I would encourage critics of *Distant Horizons* to adopt an analogous approach. Simply pointing to a list of errors in the metadata will prove nothing. (We already know there are thousands of errors.) To demonstrate that the book's conclusions need revising, a critic would ideally measure the level of error, and show that errors are distributed in a way that could have significant consequences for specific conclusions advanced in the book.

Dependencies
------------

Mainly, Python 3 and R. Within Python 3, the usual packages (numpy, pandas, scikit-learn); within R, the usual packages (ggplot2, dplyr, scales). 

There are also a lot of rule files I invoke from time to time, many of them [from my DataMunging repo.](https://github.com/tedunderwood/DataMunging/tree/master/rulesets) Have tried to build these into to chapter-level folders, but I may not always have succeeded.

Structure
---------

This is not a single article; it's a book covering five years of research. The data changed along the way, my assumptions about methods changed, and the goals of different chapters are in any case different. So I have not attempted a tightly unified structure, where all the chapters refer back to one central dataset. Instead, each chapter uses a slightly different sample. For a broader description of my decision not to stabilize sampling, see Appendix A itself.

Attempts to reproduce a result should ordinarily begin at the chapter level. Those chapter repos are usually organized around documentation for figures, so the easiest way to identify data and code you're interested in may be to identify a figure that used the data, and trace it backward to its sources.

I have usually provided an R script that created the figure, plus the table it immediately used. In many cases that table is itself the result of a modeling or transformation process, and I have tried to guide you to the appropriate code. But the original source data used in *that* process may or may not be (immediately) included in the repo. It's often a folder of several thousand wordcount files, which may be slightly too big to fit in the 1GB github size limits. So I have tried to provide a link to those source files, packaged up as .tar.gz in the Illinois IDEALS repository; to re-run the modeling process, you'll need to unpackage these files and place them in the folder indicated.

In some cases, though, you may be less interested in re-running the original modeling than in identifying the books used. Check the metadata folder in a given chapter repository, and/or check the code to find out which metadata file was used for the process you're interested in.

Stochastic character of modeling
--------------------------------

A lot of the models produced in this book use a Python script called versatiletrainer.py; generally they use the version in horizon/logistic, although chapter 4 actually uses its own files in the chapter4 folder. (As I mentioned above, this is not a tightly refactored structure!) 

In any case, versatiletrainer is not designed to produce the same result every time you run it. Randomness comes in when files are selected, e.g if there are more files available in the negative class than needed to match positive examples. Randomness also plays a role in the modeling process itself. So you should not be surprised if you reproduce the modeling and get a result that is .1% - .5% higher or lower than the figure cited in the book.

I thought about setting this up so it's reproducible in a more airtight way, with a random number seed so you get exactly the number I got if you use the same inputs. But there are no arguments in the book that actually depend on this level of quantitative precision, and actually I felt it was more important to acknowledge that the Brownian motion is there.

Similarly, you'll find that the coefficients produced by your models may not exactly line up with the coefficients in mine. If we were interpreting models by looking at the top and bottom ten words in the list of coefficients, this would be a problem. But that's why, in fact, the text of the book urges people *not* to interpret models with thousands of variables purely by looking at ten at the top or bottom--but to use more robust approaches.

If you want to reproduce everything from scratch
-------------------------------------------------------

The individual chapter repos are based on the assumption that you're happy to reproduce results from wordcount files, which I can legally provide zipped up as .tar.gz.

If, however, you want to take everything all the way back to the source (or just want to see how I handled feature counting), the two code directories provided here will assist you. The source files in chapter 2 were produced by **countwordsintexts/tokenizetexts.py**. The source files in chapters 1 and 3 were produced by **countwordsinfeatures/parsefeaturejsons.py**, which [works with HTRC extracted features.](https://wiki.htrc.illinois.edu/display/COM/Extracted+Features+Dataset)

The reason for the difference is that chapter 2 worked with some texts from the Chicago Text Lab, not in Hathi. At the time I did that research, Hathi hadn't yet made post-1923 texts available as extracted features.

For chapter 4, you'll need to consult [David Bamman's BookNLP itself.](https://github.com/dbamman/book-nlp), and then the chapter4/transform_data folder.

If you want a master list of English-language fiction in HathiTrust
----------------------------------

You could check out **pre1923hathifiction.csv** and/or **incopyrighthathifiction.csv** [in the NovelTM metadata repo,](https://github.com/tedunderwood/noveltmmeta) for a very inclusive list of HathiTrust volumes. I didn't use this whole list for anything, but it will give you a sense of the *outer limits* of the library, things that may be missing, etc.

For a more rigorously filtered and deduplicated list of fiction, try **filtered_fiction_metadata.csv** in [horizon/chapter4/metadata](https://github.com/tedunderwood/horizon/tree/master/chapter4/metadata).

If you want to understand how these lists were produced, try [the report for *Understanding Genre in a Collection of a Million Volumes.*](https://figshare.com/articles/Understanding_Genre_in_a_Collection_of_a_Million_Volumes_Interim_Report/1281251) It says its an interim performance report, but actually it's the final version.

If you want a master list of English-language poetry in HathiTrust
-----------------------------------

Probably the best thing to consult is [poemeta.csv.zip, in the Understanding Genre project](https://figshare.com/articles/Page_Level_Genre_Metadata_for_English_Language_Volumes_in_HathiTrust_1700_1922/1279201). That list of poetry will stop in 1922, but so does the research on poetry in *Distant Horizons.* Again, this list is meant to trace the outer limits of the poetry collection I drew from in this project; it's not a list that was itself directly used in the research.





