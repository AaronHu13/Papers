## Normalized Cuts and Image Segmentation

#### abstract

They propose a novel method for solving the perceptual grouping problem in vision, which has been mentioned both in our EECS 432 and EECS 433. They treated Image Segementation as a  graph partitioning problem.(他们提出了一个非常新奇的方法Normalized cut来处理图片分割问题-将图片分割问题转换为一个图论的问题-如何更好地分割子图,从而使各个子图内部间的相似度更高,同时降低子图之间的相似度.(这里对应课堂上讲过的一个公式)).其实在这之前已经有人提出了这个想法,即将图片分割问题转换为图论问题,但是因为没有做normalized,导致最终的结果会倾向于得到很多孤立的小子图,这不利于语义的分割.



