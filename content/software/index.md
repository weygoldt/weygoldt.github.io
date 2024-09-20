---
title: "Software projects"
date: 2024-09-20
draft: false
heroStyle: "background"
layoutBackgroundBlur: true
layoutBackgroundHeaderSpace: true
showAuthor: false
---

I really enjoy writing code, whether it is for data analysis, visualization or
to automate small tasks in my daily life. Here, you'll find a section with some
of these projects. Most of them are work in progress and I usually don't have
the time to polish them to a point where I would consider them "finished". I
just continue to develop features as I need them.

## `chirpdetector`

is a Python package that leverages YOLO models to detect electrocommunication
signals on spectrograms of terrabytes of recordings of electric fish.
The model backend is implemented as a "plug and play" system, allowing for
easy integration of new models. The main selling point of this package is the
framework to efficiently parse and process large amounts of data.

A previous prototype of this package using a simple CNN classifier was
described in this article:

{{< article link="/posts/chirp_detector/" >}}

The current version of the package is still in development, but you can check
out the repository for more information:

{{< github repo="weygoldt/chirpdetector" >}}

## `gridtools`

is a Python package that provides a data model to load, process and save
multi-channel electrode grid data including metadata and derived features, such
as the communication signals and position estimates of the fish. The data model
is designed to be easily extendable and implemented using `pydantic` to ensure
type safety. I am also working on extending simulation features to generate
synthetic data for testing and training purposes.

{{< github repo="weygoldt/gridtools" >}}

## `reportable`

is a Python package that provides a simple way to "extract" reports from larger
data science repositories. Sometimes, I find myself in a situation where I have
a report, such as a slide deck, that links to a few plots in a directory with
many other plots. This package helps me to extract only the relevant media
files and copy them to a new directory and update the links in the report. This
makes it easier to share the report with others without sharing the entire
repository.

{{< github repo="weygoldt/reportable" >}}

## `py2md`

is a Python package that converts Python files to Markdown files. Let me
explain: I hate Jupyter Notebooks. I really prefer to just have plain text
files to make version control, and file management easier. So I came up with
the idea to write my articles in Python files using block comments for text and
the rest as code. This package extracts the text from the comments and wraps
the code in Markdown code blocks. This way, I can write my articles in a single
file and convert them to Markdown. An additional benefit is that the file is
executable and easy to run, making it a self-contained and efficient solution.
This article explains the process in more detail and at the same time, is
the actual output of the package after running it on itself:

{{< article link="/posts/py2md/" >}}

Check out the repository for more information:

{{< github repo="weygoldt/py2md" >}}

## `slicepy`

creates **s**treamlined **l**ists of **i**mportant **c**ode **e**dits. During
rapid prototyping phases, I find myself writing tons of `TODO` comments which I
then forget about. This package extracts these comments along with some context
from a code base and then queries the OpenAI-API to generate a summary of every
`TODO` item. The result is a markdown file with a checklist of all `TODO` items
and their context, placed in the root directory of the project.

{{< github repo="weygoldt/slicepy" >}}

## `audioviz`

is a Python package that provides a simple way to visualize audio data. Running
it on an audio file, it creates an animated video of the spectrogram and
the audio waveform in sync with the audio track. This package is still in the
early stages of development, but I plan to extend it to include more features
as soon as I need it for presentations or blog posts.

{{< github repo="weygoldt/audioviz" >}}

## `dotfiles`

are a collection of configuration files for my Linux system. I use them to set
up new systems quickly and to keep my system configuration consistent across
different machines. This way, I can easily switch between my work laptop and my
home desktop without having to worry about different configurations. They are
highly opinionated and tailored to my needs, but feel free to use them as a
starting point for your own configuration.

{{< github repo="weygoldt/dots" >}}


