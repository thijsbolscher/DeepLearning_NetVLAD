
# NetVLAD

## Reproduction Project CS4240 Deep Learning

Xander van Beurden & Thijs Bolscher

Link to code: https://github.com/xvanbeurden/Deep_learning/blob/main/Main.ipynb

## 1 Introduction

The goal of the original NetVLAD project, was to quickly and accurately recognize the location where a picture is taken. To do so, a new generalized VLAD (Vector of Locally Aggregated Descriptors) layer was developed. This layer was named NetVLAD. This NetVLAD layer can simply be added to existing CNN structures such as AlexNet and VGG-16. For training & backpropagation to be executed, the authors came up with a smart weakly supervised ranking loss. The idea is to finally use the Google Street View Time Machine, so that locations where pictures are taken can be accurately placed over the whole world. 
This project showed really promising outcomes on two benchmark datasets, including pictures taken in the Tokyo and Pittsburgh regions. From the figure below the challenge of place recognition already becomes clear. A well-performing place recognizer should be able to look through variability such as changing seasons, night/day, construction work and much more. 

![comparison](https://user-images.githubusercontent.com/60961644/114835342-c1cb8b80-9dd1-11eb-8a4a-f7fd4d5eb3f1.PNG)

In summary, the most important building blocks of this reproducibility project are: 
- Loading in the data 
- Building the NetVLAD layer
- Using clusters and obtaing clustering centroids
- Attaching potential positives and definite negatives to all training queries based on distance in the real world
- Finding the best potential positive and worst definite negatives through comparison of the feature vectors
- Implementing the loss function and using these (Query, Pos, Neg) structure to train the network


During this project we attempted to reproduce the accuracy measurements done in the paper. Unfortunately, due to the many roadblocks we hit upon, and our lack of time and especially computing power (as we will elaborate upon) we could not go all the way. However, we were able to find some promising results during training in the first few epochs. At the end of this blog post we will briefly talk about the problems we faced due to our limited experience with the field of deep learning and building such Deep Neural Networks. 



## 2 Loading in the data
The performance of NetVLAD that we focussed on was on the Pittsburgh 250k dataset. The dataset contains of 250,000 images taken in the Pittsburgh area. It was noticed that spending some time on the data in this dataset helps understanding some of the decisions made in the paper. Empirically, it was seen that the data was best put on the runtime disk memory and the RAM of the GPU could handle the running memory. It was mentioned in the paper that the data was divided into roughly three equal parts for training, testing and validation. After some research, it became apparent that this division was made by the authors and put into a .mat file that was extracted.  So before starting off with building our own dataloader, the data was retrieved from the dataset (available online) and looked at. Next, the dataloader was made. We soon realised that the dataset was too large to handle for the limited computing power and memory we had at hand (1 GPU) so the images had were resized, unlike in the paper. As it was unclear from the paper, some time also had to be invested into arranging this dataloader function as it differed for example for the query image set versus all other images.

## 3 Building the NetVLAD layer
The building of the NetVLAD layer was quite a smooth operation. The construction of the layer is clearly explained in the paper. Moreover, for the CNNs, existing networks were directly uploaded and used as they should be. In the paper it is clearly explained what parts of the network to use and how far to train them to reach similar (accuarcy) results as the paper. 



## 4 Clustering
The concept of the centroid and clustering was for a long time a hard one to grasp for us. The author of the paper really writes from a point of view where you have experience with VLAD layers or at least tasks that are similar to this place recognition task, in which 'anchors' need to be placed in the vector space to be able to compare the descriptors of different images well. Moreover, from the paper it was not at all clear to us what exactly was this (size of the) cluster set. Out of how many and out of which query images should such a cluster consist? Although in the Appendix, the number of clusters adviced is mentioned, the whole concept of this clustering was still not clear enough to us. In the end we indeed set the number of clusters to 64, and used K-means (our decision, not mentioned in the paper as well!) to find the clusters and their centroids. Finally, it was pretty hard to find whether for us to find the clusters and their centroids, the images needed to go through the CNN only, or also the NetVLAD layer.

## 5 Potential positives and definite negatives
During this course, we were warned many times that often papers use mathiness to impress rather than to explain. In this paper, when it comes to finding the potential positives and definite negatives for each query, this was definitely the case. Many terms were used to describe the same procedure over and over and these terms were never explained in plain and clear language. The best example of this is the mentioning of 'Euclidean distance', which in hindsight always referred to the geographical distance in the real world, but could have also meant a certain distance in the vector space which was so often mentioned in the NetVLAD report. 


## 6 Best Positives and worst negatives 
This is where the vector space does clearly come in. In our opinion, the paper does a really good job at describing this process, and it is really easy to find all the hyperparameter values you need for this procedure. However, the paper does not mention what functions they use to find the most-alike and least-alike vectors in the vector space. KNN? Faiss? In most occasions we chose to use scipy's NearestNeighbors, where the more efficient faiss might save a lot of computational time. One other problem that we will elaborate upon under point 8, is that it was hard to find out where it was advisable to store variables or tensors in cache, and where not. In our opinion, it would have added a lot of value if the author would have implemented a little more information on this. At the same time we realize that within the world of Deep Learning and Computer Science, experienced coders will be able to anwer these questions themselves. With other words, it would have helped US if there was a little more basic information in the paper for us to independently reproduce it accurately, but we realize that the athor cannot describe everything in one paper.


## 7 Implementing the loss function and training
This part of the project took us quite some time to unravel - which, in hindsight, was not needed. The well-explained theory behind the loss function makes it easy to understand what the author wants, but not necessarily how to implement this in code. Turns out that Pytorch has a function called 'TripletMarginLoss' which does all the work for you, as long as you give the function the margin you have in mind, and this margin value is once again clearly mentioned in Appendix A.

During training, the model took approximately 10 hours to run an entire epoch and before the results were tested and the accuracy of our model was determined, we reached the training time limit from Google Colaboratory. However, as can be seen in our uploaded code, training happened and our loss was decreasing but after putting too much effort into decreasing training time and increasing possible running time it was too late to test the accuracy of the model at the end of one epoch. 




## 8 Us as Aerospace Engineers; the troubles we faced
Among several implementation issues we ran into, the main issue for us as aerospace students that we ran into was the model architecture. At first, we did not oversee the scale of the image dataset (205k!!) and the issues that come with it such as memory allocation and computing time. Although the authors did a good job at explaining how to handle the memory by specifying the caching details, and tuples and batch sizes, implementing was at first a black box for us. By talking with peers studying computer science and looking at related code provided by Nanne [1], it became much clearer how to implement this. This made us realize the gap between Aerospace Engineering students and Computer Science students, which was interesting to notice. As the Aerospace industry is also moving towards data-driven decision-making, our curriculum is maybe lagging a bit behind. The paper lacked however to specify the hardware used and the training times that were realized. This would have helped towards making decisions for the use of training platform we would be using to upload the data to and to train from. 
Finally, the decision was made to train the network with Google Colaboratory placing the image dataset in the runtime memory, using the wget function from Python. After resizing the images to 100x100, the model would still take 10 hours to run one epoch and as such, unfortunately no concrete results are published as Google would kick us out. We must have realized earlier in the process that it is quite easy to save the (updated parameters of the) model such that even though your model does not finish training, the latest model can be used for testing. Unfortunately, the authors made the data no longer available in the last couple of days, such that we could not downscale the number of images and rigorously decrease training time. 

Concluding, for the course it is quite interesting to look at all the different deep learning methods but without being able to handle the huge amounts of data that deep learning is usually concerned with, your network will never be able to train. As Aerospace students this was seen into practice as the paper with its methods and architecture was really clear, but we bumped into realizing the code. We realised that Google Colaboratory is a great platform to initially develop, test and debug your code on, but constructive training should be done alternatively when handling these amounts of data. 



[1] Nanne (2019), https://github.com/Nanne/pytorch-NetVlad







