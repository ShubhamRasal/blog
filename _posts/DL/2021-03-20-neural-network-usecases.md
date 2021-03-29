---
title: "What is Neural Network? What are usecases of Neural Network?"
toc: true
toc_label: "Content"
tags:
  - Deep Learning
  - Arth-Tasks
date: March 20, 2021
categories: Linux
header:
  #image: assets/images/arth-task-20
 # overlay_image: /assets/images/thumbnails/linux.png
  teaser: "/assets/images/arth-task-20/header.png"
permalink: /:categories/:title/
excerpt: "Read about neural network and applications of neural network"
comments: true
---
![header.png]({{ site.url }}{{ site.baseurl }}/assets/images/arth-task-20/header.png)

No doubt machine learning, Deep Learning and Artificial has changed every aspect of all of our lives.

> Artificial Intelligent, deep learning, machine learning - whatever you're doing if you don't understand it-learn it. Because otherwise you're going to be a dinosaur within 3 years.
> 
> \- MARK CUBAN

In this article, I will try to give overall idea of Artificial Neural Network and why and where is used in real life?

Artificial neural network may seem hard to understand, but they are the force behind some of very powerful technology.

But let first understand,

## what is neural network?

It is a set of algorithms, which are designed to mimic the human brain and find the patterns in the data. Artificial neural networks can be said as computational learning system that uses network of functions to **understand** and **translate** data input and convert into desired output. This functions interpret data through the form of machine perception by labeling raw input data. The concept of artificial neural network was inspired by human biology and the way neurons of the brain function together to understand input from human senses and convert it.

Let's take moment and make it more simpler..

Our human brain is made of network of neurons. Thus making very complex structure.

Humans are capable of see the situation, access it quickly and understand it and react it.

but, computers struggle to react to situation in a similar way. To overcome  this limitation, artificial neural network is the way.

Neural networks were first proposed in 1944 by Warren McCullough and Walter Pitts, two University of Chicago researchers who moved to MIT in 1952 as founding members of what’s sometimes called the first cognitive science department.

## How to Artificial Neural Networks Work?

ANNs are composed of multiple**nodes**, which imitate biological**neurons**of human brain. The neurons are connected by links and they interact with each other. The nodes can take input data and perform simple operations on the data. The result of these operations is passed to other neurons. The output at each node is called its**activation**or**node value.**

The neurons, within each layer of neural network, perform same function. They simply calculate the weighted sum of inputs and weights, add bias and execute an activation function.

![A Typical ANN](https://www.tutorialspoint.com/artificial_intelligence/images/atypical_ann.jpg)

Each layer has artificial neurons called units. This units allow the layer to process, categorize and sort information. They are the processing nodes and have their own piece of knowledge. Each connection is weighted and the heavier the wight, or bigger the number, the greater the influence that the unit has over other units.  This structure allows the network to learn and react both structured and unstructured information and datasets.

The first layer is input layer that takes the information in various forms. This information is progresses through hidden layers where it is processed and analysed. At the end, this data is reaches to the end of the network, the output layer. Here the network works out how to respond to the input data. This response is based on the information it has learned throughout the process.


![nn.gif]({{ site.url }}{{ site.baseurl }}/assets/images/arth-task-20/e2d85484010a474d8bc6052f77c3d47a.gif)

## Use Cases of Neural Networks 

### WaveNet: A generative model for raw audio

Allowing people to converse with machines is a long-standing dream of human-computer interaction. The ability of computers to understand natural speech has been revolutionised in the last few years by the application of deep neural networks (e.g., [Google Voice Search](https://ai.googleblog.com/2015/09/google-voice-search-faster-and-more.html)). However, generating speech with computers  — a process usually referred to as [speech synthesis](https://en.wikipedia.org/wiki/Speech_synthesis) or text-to-speech (TTS) — is still largely based on so-called [concatenative TTS](https://scholar.google.com/citations?view_op=view_citation&hl=en&user=Es-YRKMAAAAJ&citation_for_view=Es-YRKMAAAAJ:u5HHmVD_uO8C), where a very large database of short speech fragments are recorded from a single speaker and then recombined to form complete utterances. This makes it difficult to modify the voice (for example switching to a different speaker, or altering the emphasis or emotion of their speech) without recording a whole new database.

![wave.gif]({{ site.url }}{{ site.baseurl }}/assets/images/arth-task-20/fb05a3d64ee7458f919143d07dc9efad.gif)

Read More: [Wavenet paper](https://arxiv.org/pdf/1609.03499.pdf)

# Applications of artificial neural networks in medical science

Computer technology has been advanced tremendously and the interest has been increased for the potential use of 'Artificial Intelligence (AI)' in medicine and biological research. One of the most interesting and extensively studied branches of AI is the 'Artificial Neural Networks (ANNs)'. Basically, ANNs are the mathematical algorithms, generated by computers. ANNs learn from standard data and capture the knowledge contained in the data.

Neural networks are ideal in recognizing diseases using scans since there is no need to provide a specific algorithm on how to identify the disease. Neural networks learn by example so the details of how to recognize the disease are not needed. What is needed is a set of examples that are representative of all the variations of the disease. The quantity of examples is not as important as the ‘quantity’. The examples need to be selected very carefully if the system is to perform reliably and efficiently.

Nowadays, ANNs are widely used for medical applications in various disciplines of medicine especially in cardiology. ANNs have been extensively applied in diagnosis, electronic signal analysis, medical image analysis and radiology. ANNs have been used by many authors for modeling in medicine and clinical research. Applications of ANNs are increasing in medical industry and medical data mining.

n. ANNs hold a great deal of promise for modeling complex tasks in process control and simulation and in applications of machine perception including machine vision and electronic nose for food safety and quality control.[](https://pubmed.ncbi.nlm.nih.gov/15066233/)

### [Artificial neural networks for predictive modeling in prostate cancer](https://pubmed.ncbi.nlm.nih.gov/15066233/)

### [Automated detection of COVID-19 cases using deep neural networks with X-ray images](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC7187882/)

### **Facial Recognition Software**

Technology companies have long been working toward developing reliable facial recognition software.One company leading the way is Facebook.

For a number of years now they have been using the facial recognition technology to auto-tag uploaded photographs.

They have also developed DeepFace.

![2b741d217fdfebb9380e997d9ff93f4d.png]({{ site.url }}{{ site.baseurl }}/assets/images/arth-task-20/68e668d49d3847459813db6e0315f3a9.png)

Source -MIT

During the training process, tests were carried out presenting the system with side-by-side images.The system was then asked to identify if the images are of the same person.In these tests, DeepFace returned an accuracy rating of [97.25%](https://www.forbes.com/sites/amitchowdhry/2014/03/18/facebooks-deepface-software-can-match-faces-with-97-25-accuracy/#67cfd78b54fc).Human participants taking the same test scored, on average, 97.5%.Facebook has also taken its software to [computing and technology conferences](https://www.technologyreview.com/s/525586/facebook-creates-software-that-matches-faces-almost-as-well-as-you-do/).This is done with the purpose of allowing academics and researchers to assess and inspect the technology.With all this work it’s little wonder that DeepFace may be the most accurate facial technology software yet developed.

### **Artificial Neural Networks are Revolutionising Business Practises**

Artificial Neural Networks may be a complex concept to fully understand.However, by using them in conjunction with deep learning tools allows computer-driven technology to make gigantic leaps forward.From streamlining manufacturing to product suggestions and facial scanning, Artificial Neural Networks are transforming the way businesses operate.

## Resources

1.  [algorithmxlab.com](https://algorithmxlab.com/blog/10-use-cases-neural-networks/#:~:text=Different%20Types%20of%20Neural%20Networks,networks%20have%20greater%20learning%20ability.)
2.  [kdnuggets.com](https://www.kdnuggets.com/2019/07/neural-code-facebook-uses-neural-networks.html)
3.  [ifthen.ai](http://www.ifthen.ai/2019/05/01/5-use-cases-for-neural-networks/5143/)
4.  [pubmed.ncbi.nlm.nih.gov](https://pubmed.ncbi.nlm.nih.gov/15066233/)

Arth Task 20 Completed
# Thank you…

*About the writer:*
*Shubham** loves technology, challenges, continuous learning, and reinventing himself. He loves to share his knowledge and solve daily problems using automation.*
