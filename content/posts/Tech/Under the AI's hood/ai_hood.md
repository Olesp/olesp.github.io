---
title: "How does AI work ?"
date: 2023-02-13T08:06:25+06:00
description: How does AI work ?
hero: images/vault.png
menu:
  sidebar:
    name: How does AI work ?
    identifier: aihood
    parent: technical
    weight: 10
tags: ["AI", "Multi-langues"]
categories: ["Tech"]

---

AI, which stands for Artificial Intelligence, is a branch of computer science that aims to develop machines capable of performing tasks that typically require human intelligence, such as speech recognition, decision-making, and visual perception. At its core, AI involves the use of algorithms, which are a set of instructions that guide a computer's actions.

Machine Learning is a particular subfield of AI that uses algorithms to learn from data and improve performance on tasks over time.

To gain a basic understanding of how machine learning works and how to evaluate it without a computer science background, read on to uncover this essential component of AI and see past its apparent human-like intelligence.

##  What happens under the hood?

Machine learning is an algorithm that allows a computer to learn from data without being explicitly programmed.

### Step 1: Input & output

This algorithm takes inputs, performs calculations on them, and produces an output. When creating a model, we first need to have a question in mind that we are trying to answer.

For example, let’s say we have images of hand-written numbers (“0”, “1”, “2”, etc.) and we want to identify the digit shown. The input to the model would be one of these images, and the output would be its guess of which number is written.

> How does a machine learning model calculate without a formula?

It would be easy to write a formula which describes the relationship between speed, distance, and time. Speed x time = distance travelled.

It would be extremely difficult to write a similar formula for the relationship between the values of all the pixels in an image and the number being represented in that image. Think of how many ways each number can be written.

Instead of creating an explicit, calculations are created which convert the input format to the output format e.g. processing a picture into a single digit, or converting weather data into a percentage representing the likelihood of rain the next day. These calculations can then be tweaked to create a different output (of the same format).

### Step 2: Training for the behaviour we want

When a model is initially built to accept inputs, it will not have the correct logic to produce labels. If we give it inputs, the output will be a random figure from the values we allow as an output (in this case, the digits 0–9).

To get the right behaviour, the model is trained; it is shown examples of the behaviour we want so that it can spot the trends.

A model is trained using training data; sets of inputs where each input is attached to its correct output. The model then looks at the input and produces a guess. Based on how far its output is from the correct output, it will tweak* the way it does its calculations. A new input from the training set is passed in, and the process is repeated.

>💡 The model’s tweaking is behaviour that is programmed by the developer. Training is a tricky process with lots of parameters which need to be set correctly. If done incorrectly, it may never learn the behaviour we need.

### Step 3: Testing performance

Once the system has been trained, it can then be tested on a new, unlabeled image of a handwritten digit, and the system will use its learned relationship between the images and the labels to predict the label for the new image. We can find the accuracy of the model by comparing the labels of the testing data to the labels the model produces.

### Step 4: Using the model

That’s it?

Yep. Even cutting-edge ML models follow this structure, although they are far more advanced in how they process inputs and outputs, and in how they run their calculations.

## The pitfalls of AI

While AI has many potential benefits, it also has several drawbacks and weaknesses that must be addressed.

### Garbage in, garbage out

AI systems can only be as good as the data they are trained on. If the training data is biased or incomplete, the AI system may make unfair or inaccurate decisions. Imagine there’s an AI to detect facial expressions, but it was only trained on the faces of one race. This model will likely perform poorly when being used by people of any other race.

This problem highlights the importance of ensuring that the training data used to develop AI systems is diverse, comprehensive, and unbiased.

### No explicit programming means no transparency

It can be difficult to understand how an AI system is making its decisions. Unlike a human, who can explain their thought process, AI systems can be opaque and it can be challenging to determine why they reached a particular conclusion.

### AI has no situational awareness

AI systems can be vulnerable to hacking and other types of malicious attacks, as they may not be able to detect or respond to threats in the same way a human would. This raises important security and privacy concerns that must be addressed to ensure that AI systems are safe and secure.

### AI could lead to job loss

While AI has the potential to automate many tasks and make our lives easier, it also has the potential to displace human workers and contribute to job loss.

