---
layout: faq
title: Frequently Asked Questions
permalink: /docs/faq/index.html
---

On this page, we try to list common questions and the solution. It can very well be that you experience a bug that is not listed here. In this case, please let us know ('[How do I report an issue or bug I got while using Mars?](#report)').

#### How do I install Mars?
We created a detailed [installation guide](https://duderstadt-lab.github.io/mars-docs/install/) on this website as well as a [video guide](https://www.youtube.com/watch?v=Ljzmbt1NG0w) on YouTube.

#### Where do I find the files for the tutorials and examples?
Most of the tutorial files can be found on our [Mars tutorial](https://github.com/duderstadt-lab/mars-tutorials) GitHub page.

For the FMT example, you can find the scripts on the [FMT tutorial github](https://github.com/duderstadt-lab/fmt-tutorials) including the archive and a truncated video. The full dataset is saved on [zenodo](https://zenodo.org/record/3786442).

#### How can I contact you?
The contact details you can find on our [lab website](https://duderstadtlab.org/contact/) or on our [institute website](https://www.biochem.mpg.de/duderstadt)

#### Why didn't anything open when I ran the command to create a Molecule Archive?
Always check the console for more information (Windows->Console). If you had the wrong settings for a command it will display 'failure' and usually, an error message will pop up. In the console, error messages from Fiji will be displayed.

If you are a Mac user please verify if JavaFX is installed in your Fiji. To check this, show the package contents of the Fiji folder by right-mouse-clicking on the Fiji folder and selecting ‘Show Package Contents’. Check inside the java folder and macosx subfolder. If you see ‘adoptopenjdk-8.jdk’ in the folder then you do not have JavaFX installed. In this case you will need to replace this jdk file with a ‘zulu_8.jre’ for the Mars interface to work. You can download the jre file here. Unzip the downloaded file and replace ‘adoptopenjdk-8.jdk’ with ‘zulu_8.jre’. Re-open Fiji after making this change. Alternatively, you can download a new copy of Fiji, which now comes with the zulu jdk with JavaFX.

#### Why do I get the error 'statement cannot begin with '<' in line 8:' when trying to run the ActionBar and scripts from mars-tutorials?
This error is caused by unrecognized syntax in the scripts or ActionBar layout files. This error occurs when the files from mars-tutorials are downloaded from GitHub in html format. See [this](https://forum.image.sc/t/help-required-fiji-macro-error-statement-cannot-begin-with-in-line-8-doctype-html/68943/2) post on the forum for further details.
<p align='center'>
  <img src='{{site.baseurl}}/docs/faq/img/line8-error.png' style='width: 500px'>
</p>  
  <div style='width: 450px; text-align: center;'></div>

We recommend using `git clone https://github.com/duderstadt-lab/mars-tutorials.git` on your local machine to download the mars-tutorials repository. This method ensures all the files are downloaded in the correct format. Downloading the larger Molecule Archives will require installing git lfs. Otherwise, the mars-tutorials can be downloaded directly from the GitHub website in zip format to avoid this problem.

#### <a name="report"></a>How do I report an issue or bug I got while using Mars?
In this case, you can create an issue on one of the Mars GitHub [repositories](https://github.com/duderstadt-lab) or post your question on the [ImageJ forum](https://forum.image.sc/tag/mars) (do not forget to tag it with 'Mars' and @karlduderstadt). We are happy to help!

<p align='center'>
  <img src='{{site.baseurl}}/docs/faq/img/trouble.png' style='width: 950px'>
</p>  
  <div style='width: 450px; text-align: center;'></div>
