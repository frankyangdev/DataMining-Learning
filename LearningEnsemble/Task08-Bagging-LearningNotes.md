### Bagging ###


**Bagging** (short for **bootstrap aggregating**) is a technique for reducing generalization
error by combining several models (Breiman, 1994). The idea is to
train several different models separately, then have all of the models vote on the
output for test examples. This is an example of a general strategy in machine
learning called **model averaging**. Techniques employing this strategy are known
as ensemble methods.

The reason that model averaging works is that different models will usually
not make all the same errors on the test set.


![image](https://user-images.githubusercontent.com/39177230/115105133-81197100-9f8f-11eb-9e13-07177ddac74c.png)

A cartoon depiction of how bagging works. Suppose we train an 8 detector on
the dataset depicted above, containing an 8, a 6 and a 9. Suppose we make two different
resampled datasets. The bagging training procedure is to construct each of these datasets
by sampling with replacement. 

The first dataset omits the 9 and repeats the 8. On this dataset, the detector learns that a loop on top of the digit corresponds to an 8. 
On the second dataset, we repeat the 9 and omit the 6. In this case, the detector learns that a loop on the bottom of the digit corresponds to an 8. 
Each of these individual classification rules is brittle, but if we average their output then the detector is robust,achieving maximal confidence only when both loops of the 8 are present.
































### Reference ###

1.  花书 Deep Learning (Ian Goodfellow ,Yoshua Bengio, and Aaron Gourville) - chapter 7.11 Bagging and Other Ensemble Methods


