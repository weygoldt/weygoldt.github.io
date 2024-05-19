---
title: "Labeling images with Label Studio"
summary: ""
#showSummary: true
categories: ["Post"]
tags:
  [
    "deep-learning",
    "label-studio",
    "yolo",
    "dataset",
    "supervised-learning",
    "datascience",
    "computer-vision",
  ]
date: 2023-11-10T15:34:03+01:00
draft: false
---

_Note: This guide was written using label-studio version `1.8.2.post1`._

With the modules included in `pytorch`, `opencv` or `ultralytics`, training computer vision models is easier than ever, the main constraint being the availability of labeled data. While there are many "smart" labeling solutions, most of them are either expensive or not open source. [Label-studio](https://labelstud.io/) is unique in that it free to use, open source and even can be made "smart" by adding your own pre-labeling network for automatic labeling.

**But** setting up label-studio locally to correct pre-annotated images of my YOLO dataset that I passed through a Faster R-CNN model trained on synthetic data took me about two days. So I decided to write this in case I forget how I did it. And maybe it helps someone else before they waste two days of their life.

## 1. Convert the dataset to label studio format

First of all, you need a dataset in the YOLO format, which is structured like this:

```python
dataset
├── classes.txt
├── images
│   ├── image1.png
│   ├── image2.png
│   └── ...
└── labels
    ├── image1.txt
    ├── image2.txt
    └── ...
```

Label studio cannot directly import a YOLO dataset that is labeled. It could import the images only, but for the labels to be loaded as well, we need to convert them to the label-studio `json` format.

This can be achieved by the `label-studio-converter` tool, which comes with label studio.

The converter needs 4 parameters:

- `--input`: The path to the dataset, i.e. in our case that would be `/home/user/data/dataset`

- `--output`: The path to the output file, i.e. `/home/user/data/dataset/output.json`. To make life easier, just put it into the dataset root directory.

- `--image-root-url`: Now this is the tricky one: The root URL is now a **relative** path. Don't ask me why, but it needs to be relative to the **parent** of the root URL exported as our **environment variable** (as you will see later). And to make life even harder, it needs to be prefixed with `/data/local-files/?d=`. So in our case, the root URL would be `/data/local-files/?d=dataset/images`. Label studio will serve the images from the root URL and the images are located in the `images` folder of the dataset.

- `--image-ext`: The file extension of the images. In our case, that would be `.png`.

And to assemble the full command:

```sh
label-studio-converter import yolo -i /home/user/data/dataset -o /home/user/data/dataset/output.json --image-root-url "/data/local-files/?d=dataset/images" --image-ext .png
```

This will write two files into the dataset directory: `output.json` and `output.label_config.xml`. The latter one contains the label format that we need to import the dataset into label studio.

## 2. Export environment variables

To label local images, label-studio needs to be launched with two environment variables set. The first one enables local file serving and the second one sets the document root.

The document root **must** be an absolute path and the direct parent of the dataset directory. Example: If the root URL is `/home/user/data` then the dataset, itself containing an image and label folder, must reside in e.g. `/home/user/data/dataset`.

```sh
export LABEL_STUDIO_LOCAL_FILES_SERVING_ENABLED=true
export LABEL_STUDIO_LOCAL_FILES_DOCUMENT_ROOT=/home/user/data
```

To make this easier, I put this into a small bash script that exports the variables and launches label studio:

```sh
#!/bin/bash
export LABEL_STUDIO_LOCAL_FILES_SERVING_ENABLED=true
export LABEL_STUDIO_LOCAL_FILES_DOCUMENT_ROOT=/home/user/data
label-studio
```

Just do a `chmod +x` on the script and you can start label studio with `./label-studio.sh`.

## 3. Create a new project in label studio

Open label-studio and create a new project.

1. Give your project some name.
2. Paste the content of the `output.label_config.xml` file into the label config field.
3. Go to the settings and add a Cloud Storage, choosing Local Storage as the type.
4. Set the storage root URL as an absolute path to the root of your dataset, i.e. `/home/user/data/dataset`.
5. Now, import the `output.json` file into the project.

You should now have the labels and images loaded into label studio.

## 4. Label the images

Your project should now be ready to correct the labels. You can now start labeling the images. Once you are done, you can export the labels using the export button back into the YOLO format. This will produce a zip file containing the images and the labels. Unzip it and you are back where you started just with hopefully better labels.

I hope this helps someone. If you have any questions, feel free to contact me.

