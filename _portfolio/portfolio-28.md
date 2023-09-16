---
title: "Environmental Context Classification"
excerpt: "
* Trained a natural language model for autonomous vehicle environmental context classification using <b>Python & TensorFlow/Keras</b><br> 
<img src='/images/route-280.png' width=300px>"
collection: portfolio
permalink: "/portfolio/context-classification/"
date: 2023-03-30
published: true
tags:
  # - Machine Learning / AI
  # - Optimal Control
  # - Optimization
---
## Key Takeaway

Developed a sensor-agnostic environment context classification model which leveraged natural language 
to determine whether an autonomous vehicle was in urban, sub-urban, rural or open-sky environments. 

<!-- ## Summary
The project was borne from a research effort involving simulation-based testing of autonomous vehicle policies.
However, even if a simulation environment has high fidelity, we cannot naively assume failures happen with the 
same frequency or impact as real-life. To better steer the simulation environment to generate salient and 
relevant failures in a way consistent with real life, we wanted our sensors to observe environment features 
in the same way they would in real life. For example, blurry camera lenses on a rainy day or more noisy LiDAR
data profiles around more reflective objects -- a data-driven conditional observation model. 

While this is important for testing the policy, I was also interested in how we can train more intelligent policies.
For example, policiies which aer more flexible and can adapt based on their surroundings. The issue of environment
characterization is an important one for autonomous vehicle polciies. If you know you are in an environment where
you expect degradation of a certain sensor, you should be better positioned to decrease the weight of that sensor's
reading in your fusion algorithms. Sensor fault detection and exclusion. 

A problem with current environment characterization works at the time was they were very sensor-dependent. For example, 
GNSS environment characterization works involved only GNSS characteristics such as C/N0 or #satellites. This work
aimed to be sensor agnostic by relying only on natural language which is actually quite attainable especially for 
works which deal in scene segmentation and object detection of which there are many. It ultimately came down to a 
simple hypothesis which was to take in a bag of words output from your favorite scene segmenter and object detector
and train a neural network to match those words and output a distribution over target environment classes.  -->

## Summary
The project was borne from a research effort involving simulation-based testing of autonomous vehicle policies.
However, to better steer the simulation environment to generate salient and relevant failures in a way consistent with real life, a data-driven conditional observation model was needed. I also became interested in how we can train intelligent AV policies which are able to adapt to their surroundings and investigated the literature to discover a need for sensor-agnostic environment characterization. 

## Method

The approach relies heavily on the OpenStreetMap (OSM) open-source mapper which provides geographic information using a specialized .osm format. This file describes an environment map elements using nodes, ways and relations all of which typically contain tag keys and values. These keys and values describe the features of the map elements. 

### Context Modeling
The context associated with a data-point is defined as follows:
<p align = "center">
  <img src='/images/context-defn.png' width="50%"> 
</p>
  * For a given datapoint, a 100m x 100m square area around the point is bounded. 
  * Map data describing that specific area is obtained by querying the OSM mapper. 
The tag keys used are given and defined below: 

| Tag Key          | Definition                             |
| --------         | ------------------------------------------------------------ |
|  highway | Used for roads and road-related facilities. |
| natural | Used to describes natural physical land features, including ones that have been modified or created by humans. |
| covered | A property to denote if an object is covered by something. |
| surface | Describes the surface of a feature. |
| sidewalk | Indicates the presence or absence of a sidewalk (pavement/footway). |
| water | Specification of a water body. |
| lanes(N) | Total number of traffic lanes available for motorised traffic. |
| capacity(N) | Describes the capacity a facility is suitable for. |
| maxheight(N) | The legal maximum height. |
| building:levels(N) | The number of above ground levels in a building (if there's only the ground level, this is 1), not including the levels in the roof. |
| roof:levels(N) | To mark number of roof levels. 
| building:min_level(N) | For describing number of values, "filling" space between ground level and bottom level of building or part of building. |

### Classifier Model

The model is composed of a Natural Language Processing (NLP) based component for discovering relationships and important features of the environment described as words; and a second component modeled as a sequence of fully connected layers for processing numeric characteristics of the environment such as average building height and number of lanes describing the road.

The text-based component is modeled as an NLP machine learning model where the inputs to the model are the text-based features of the environment. These are a subsampled set of descriptors used by the OSM mapper. The original list contains over 5000 keys and each key has a corresponding set of values they can take on to describe a feature. Features are constructed by concatenating tag values and keys for text-based characteristics.

<p align = "center">
  <img src='/images/feature_constructor.png' width="100%"> 
</p>

The architecture includes an embedding layer converting word sequences of some maximum length into a fixed size array of numbers. The embedding layer is followed by a series of fully connected layers finding relationships between words in the sequence and finally outputting a state of size 128 which encapsulates the word sequence.

The model architecture for the numeric processing is chosen to be a sequence of fully-connected layers with $tanh$ activations. For proper conditioning, numbers are rounded to the nearest integer and the absence of values is modeled with a value of zero. 

### Data Collection

We created custom training data and collect OSM data for multiple regions across the United States which represent the different contexts of interest. Obtaining Points involved hand picking specific locations or regions which we believed fell into one of the contexts of interests and then randomly sampling a fixed number of points within a one-mile radius around the origin point.

<p align = "center">
  <img src='/images/urban_canyon.png' width="100%"> 
</p>

To map our randomly generated points to points which are on the road and not, for example, on top of building the Google Maps Roads API was utilized which performs this exact function for a sequence of 100 points.

The map data obtained results in an unordered set of features which would not be much use without some initial preprocessing. This feature set has a few deficiencies including repeated and redundant feature descriptors, inane feature descriptors which don't help the model discriminate between contexts, as well as general issues common to text-based datasets including wanton punctuation usage and inconsistent key-value pairings. 

### Model Training and Evaluation

The output of the two component alpha and numeric models are concatenated and fed through a single dense layer and a dropout layer followed by a final Softmax layer for classification into one of multiple contexts.

As a multi-class classification task, the model is trained using a Categorial Cross-Entropy training loss and uses the Adam optimizer:
\begin{equation}
    \mathcal{L}(y, \hat{y}) = \sum_i -y_i \log \hat{y}_i
\end{equation}

![](/images/model_training_loss.png) | ![](/images/urban_canyon_suburban_rural.png) 

The figures demonstrates the training performance for the model when trained in the 2-, 3- and 4-class situations. Although we observe the performance degrade somewhat as more classes are addeed, the overall performance remains quite high for all showing that the model framework performs as expected and can easily be extended to deal with multi-class situations. 


