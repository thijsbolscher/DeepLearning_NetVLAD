###NetVLAD
##Reproduction Project CS4240 Deep Learning

Xander van Beurden & Thijs Bolscher

##1 Introduction
The goal of the original NetVLAD project, was to quickly and accurately recognize the location where a picture is taken. To do so, a new generalized VLAD (Vector of Locally Aggregated Descriptors) layer was developed. This layer was named NetVLAD. This NetVLAD layer can simply be added to existing CNN structures such as AlexNet and VGG-16. For training & backpropagation to be executed, the authors came up with a smart weakly supervised ranking loss. The idea is to finally use the Google Street View Time Machine, so that locations where pictures are taken can be accurately placed over the whole world. 
This project showed really promising outcomes on two benchmark datasets, including pictures taken in the Tokyo and Pittsburgh regions. From the figure below the challenge of place recognition already becomes clear. A well-performing place recognizer should be able to look through variability such as changing seasons, night/day, construction work and much more. 

![Image](/images/comparison.png)

In summary, the most important building blocks of this reproducibility project are: 
- Loading in the data 
- Building the NetVLAD layer
- Using clusters and obtaing clustering centroids
- Attaching potential positives and definite negatives to all training queries based on distance in the real world
- Finding the best potential positive and worst definite negatives through comparison of the feature vectors
- Implementing the loss function and using these (Query, Pos, Neg) structure to train the network


During this project we attempted to reproduce the accuracy measurements done in the paper. Unfortunately, due to the many roadblocks we hit upon, and our lack of time and especially computing power (as we will elaborate upon) we could not go all the way. However, we were able to find some promising results during training in the first few epochs. At the end of this blog post we will briefly talk about the problems we faced due to our limited experience with the field of deep learning and building such Deep Neural Networks. 


##2 Building the NetVLAD layer

##3 Clustering

##4 Potential positives and definite negatives

##5 Best Positives and worst negatives

##6 Implementing the loss function and training

##7 Us as Aerospace Engineers; the troubles we faced










## Welcome to GitHub Pages

You can use the [editor on GitHub](https://github.com/thijsbolscher/DeepLearning_NetVLAD/edit/gh-pages/index.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/thijsbolscher/DeepLearning_NetVLAD/settings/pages). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://support.github.com/contact) and we’ll help you sort it out.
