---
title: "Identifying Windows 10 Developer Devices"
layout: post
date: 2018-06-27
img: devdevice-front.jpeg
#tag:
#- Employment
#- Technology
#category: blog
#author: juntaeklee
description: Identifying Windows 10 Developer Devices
# jemoji: '<img class="emoji" title=":ramen:" alt=":ramen:" src="https://assets.raw.githubusercontent.com/images/icons/emoji/unicode/1f35c.png" height="20" width="20" align="absmiddle">'
---

*Disclaimer: this project was done during my employment at Microsoft. In compliance with non-disclosure agreements, specific details have intentionally been left ambiguous.*

## Table of Contents
1. [Project Brief](#project-brief)
2. [Related Work](#related-work)
3. [Methodology](#methodology)
4. [Results and Discussion](#results-discussion)
5. [Conclusion](#conclusion)
6. [References](#references)

## Project Brief

"Developer" is a ubiquitous term in the technology industry, but its operational definition is ambiguous. Accurate identification of developer devices is a prerequisite to encourage and measure developer engagement with Microsoft hardware, software and services. The existing approach to identify developer devices relies on a limited, hand-crafted list of the most popular developer tools. This approach is not always correct because some of the developer tools are also used for purposes other than software development, implying the need for a more sophisticated approach to identify developer devices. We performed a binary classification experiment to classify whether a Windows 10 device is a developer device using a feature set curated from surveys and telemetry data. This classifier outperforms precision of the naïve model which predicts all devices that use hand-crafted list of the most popular developer tools as developer devices.

## Related Work

There has been extensive research on estimating global software developer population.

### SlashData

SlashData has a conservative estimate of just under 15 million active software developers in the world in 2017, out of which 11 million are professional software developers, and the remaining are hobbyists and students<sup>[1](#1)</sup>.

### GitHub Inc.

According to GitHub Inc.'s annual report on its ecosystem, titled “The State of the Octoverse 2017”, there are 24 million developers across 67 million repositories<sup>[2](#2)</sup>.

However, none these reports and studies provide device counts and only some of them provide estimated percentage for developers’ primary operating systems used for development activity. This lack of information introduces uncertainty about developer activity on Windows 10. According to Stack Overflow Developer Survey Results 2018<sup>[3](#3)</sup>, *Visual Studio* and *Visual Studio Code* are the most popular developer environments among their respondents (69.2% of the respondents use them). However, 30.8% of their repondents indicate that they use non-Microsoft developer tools, e.g. *Notepad++*, *Android Studio*, and *Vim*. This encouraged us to perform a study to calculate accurate counts of developer devices running Windows 10.

Currently, a Windows 10 device is marked as a developer device if telemetry indicates usage of tools from a predetermined list. This implementation assumes two major points: only a trivial percentage of developer devices show usage of developer tools outside of the predetermined list for development, and non-developer devices do not show usage of applications in the predetermined list. If the assumptions are false, the first assumption leads to an underestimation of developer devices and the second assumption leads to an overestimation. With telemetry alone, the extent to which either assumption affects the distribution is unverifiable. From anecdotal information we know non-developers use some developer tools, like *Notepad++* and *Visual Studio Code* to take notes, and developers use niche IDEs. The existing approach is arguably scalable but has known limitations. This implies the need for a more sophisticated approach to identify developer devices.

## Methodology

We proposed to train a supervised machine learning classifier to identify developer devices. The model incorporated features regarding device hardware and system information, and interactivity duration for the most commonly used applications. We hypothesized that the additional features and quantitative durations of application usage will distinguish developers from non-developers more accurately than the current implementation. Moreover, through exploratory data analysis, we examined assumptions and limitations of both implementations.

### Data

We used telemetry data, census survey data on a specialized sample of Windows users provided by our marketing team, and a secondary post hoc survey from a generalized sample of Windows users to better understand performance and generalizability of our model.

### Exploratory Data Analysis

As previously mentioned, the current implementation makes two major assumptions that may drastically affect how accurately we identify developers, and future models are nonetheless susceptible to them. By cross-validating insights between the objective telemetry data and subjective survey responses, we learned about the shortcomings of both kinds of data and how they affect our proposed implementation. We examined the usage pattern of developer tools amongst our population and concluded that self-reported developers might be leveraging other operating systems to build applications.

The census survey asked self-identified developers their primary development environments for which, unsurprisingly, most indicated usage of specified IDEs. However, only a fraction of self-identified developers showed nonzero interaction durations with IDEs or text editors they identified based on telemetry data. This indicates that the remaining users were either using unmapped (multiple) Windows 10 devices or that they are using operating systems other than Windows 10 (Windows 10 may be used for personal reasons). 

Exploring whether non-developers also use developer tools, we find that a small but significant percentage show nonzero interaction duration. *Notepad++*, *Visual Studio Code*, and *Sublime Text* are easy to use and lightweight applications for note taking. By contrast, the other applications are heavy IDEs that require large memory size and high processing speed from the physical computer. Since these users claim not to do any programming, the IDEs must also have additional non-technical uses, such as technology reviews which is a common scenario for tracking work item status, for example, at *Microsoft Visual Studio Online (VSO)*.

In addition to developer tools, a sizable percentage of non-developers use Developer Mode, an option in Windows 10 designed to provide access to advanced development features<sup>[4](#4)</sup>. Since developer mode is not a default setting for devices, non-developers must have other uses for unlocking developer mode. For example, enterprises can enable developer mode to enforce their policies. Since not all development activities require enabling Developer Mode, many developers left this setting as Disabled. These findings are further evidence of the ambiguity when identifying developers simply from usage of developer tools.

### Machine Learning Experiments

We performed a series of Machine Learning (ML) experiments to predict whether a device was a Developer's device or Non-developer's device. We evaluated 12 learners which we believed could work well on our data set. In our first set of experiments, we partitioned labeled data into a training and testing set. We iteratively improved model performance by performing feature engineering and parameter sweeping. We performed a 10-fold cross-validation to assess the generalizability of our model. We used AUC, precision, and recall to measure model performance. 

## Results and Discussion

From machine learning evaluation, we found that gradient boosting learners (XGBoost, Light GBM, and FastTree) generally provided the best results. Despite getting high AUC, precision, and recall, detailed investigation of predicated labels revealed that the classifier favored devices used by Developer and IT Pro personas. It performed very well on devices with at least some interactive usage minutes of popular IDEs and developer tools like *Visual Studio*, *Eclipse*, *Notepad++*. The classifier also mis-classified retail developer devices that are occasionally or rarely used for development purpose; they utilized fewer popular developer tools. Comparing our results to the *Most Popular Development Environments* chart of *Stack Overflow Developer Survey Results 2018*<sup>[3](#3)</sup>, we estimated that missing population to be a miniscule percentage of all developer devices.

One of the key benchmarks we used was precision of the model compared to precision of the naïve model which predicts all devices that use a hand-crafted list of the most popular developer tools as developer device. This is important because the naïve model generates tens of thousands of false positives due to some code editors being used for purposes other than software development. We estimate false positives of naïve model inflates overall developer device count in all Window 10 devices by a few million.

We tested our model trained on over 10 months of data to see how well the model performed over distinct data sets and over time. The variations on model performance for precison, recall, and accuracy were miniscule. F1 score of the model showed a slightly declining trend as months passed. This may be due to new apps introduced into the ecosystem; however, in production, a model would be retrained more often, especially if the population inputs shift over time.

Another benchmark we used to understand generalizability of the model was its performance on the generalized sample of Windows 10 users. As expected our classifier predicted developer devices with high precision. However, recall rate was surprisingly low. When we used the naïve app list classifier on the same data, we received a similarly high precision and low recall value but lower than our model.

Performing cross validation on the generalized sample data itself yielded similar results. Given a high “Yes” response rate the from survey compared to our estimate of the percentage of developers, combined with low recall values from our experiments were good indicators of response bias.

Based on data provided by SlashData, GitHub, and StackOverflow, we initially estimated that developer devices would be between 2% and 3% of all Windows 10 devices with greater than 0 interactive usage time report. Early results from large scale production of this model are very encouraging as the total number of devices predicted as Windows 10 developer devices stands within our estimated range.

## Conclusion

This work builds a stronger foundation to begin identifying developer devices based on Windows 10 Telemetry data. While the previous implementation was created through intuition with no source of ground truth, the use of survey responses as labeled dataset provided opportunity for a ML classifier to more accurately predict the device class.


## References

<sup>0</sup> Cover image from [Make Tech Easier](https://www.maketecheasier.com/programming-laptop-guide/). No copyright infringement is intended.

<a name="1"><sup>1</sup></a> "The Global Developer Population," SlashData, 2017. [Online]. Available: <https://www.slashdata.co/reports/>.

<a name="2"><sup>2</sup></a>  "The State of the Octoverse," GitHub Inc., 2017. [Online]. Available: <http://octoverse.github.com>.

<a name="3"><sup>3</sup></a>  "Stack Overflow Developer Survey," Stack Exchange Network, 2018. [Online]. Available: <https://insights. stackoverflow.com/survey/2018>.

<a name="4"><sup>4</sup></a>  "Enable your device for development," Microsoft, 2017. [Online]. Available: <https://learn.microsoft.com/en-us/windows/apps/get-started/enable-your-device-for-development>.


