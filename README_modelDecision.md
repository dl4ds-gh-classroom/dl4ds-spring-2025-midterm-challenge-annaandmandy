# Hsiang Yu Huang Midterm Analysis

## SimpleCNN - Part1
### Performance
0.39479
### layer setting
#### Convolutional Layer
From RGB color input channels 3 extend to output 64 features, following ResNet's design which demonstrates better performance with 64 initial channels. Through testing, using 64 channels has a better performance. I then normalized, ReLU and dropout to train better and avoid overfitting. I used 3 layers, which extend the channels to 64, 128, 512.
I tried to extent to output 32 features first, but 64 features works better.
#### Fully Connected Layer
Use Linear Regression to compress the channels, and dropout(0.2) and finally output as 100, which is the number of different kinds of CIFAR100 type.
I tried to dropout(0.5), without dropout and dropout(0.2) when testing.
### data transforming
I used transforms.RandomHorizontalFlip() and transforms.RandomCrop(32, padding=4) to augment data, which enables the training data to show more details and handle different situations. For the normalization part, I used CIFAR100 mean and std.
### parameters setting
#### epochs = 100
setting a bigger epoch can reach a higher accuracy. Epoch 100 is overfitting when using a pretrained model, but still got the best score. I tried to lower it down to 50, while the overfitting issue is lower, but still occurs. As the result shown on wandb, I need at least 30 epochs to acheive a stable loss.
#### batch size = 128
Through runing the find_optimal_batch_size, the best batch size is 512. However, through my own testing, batch size 128 has a better accuracy.
I tried 56,128,256,512 when testing.
#### learning rate = 0.01
I tried to set the learning rate as 0.01, 0.001 and 0.0001.
Through my own testing and the result shown on wandb website, using 0.01 learning rate can quickly lower down the val_loss and reach a higher accuracy in both train and val. 
### optimizer and scheduler
I used SGD as optimizer and CosineAnnealingLR as scheduler. 
SGD performs better than Adam when I was testing. 
CosineAnnealingLR adjusts the learning rate smoothly when it trains, it have better performance. I also tried StepLR(30) and MultiStepLR[30,60,90].

## Pretrain Model Part 2
I used ResNet50 as the base model and set the pretrain to false. 
### Performance
0.46827
### Adjustment
In order to fit the CIFAR dataset, I set conv1 kernel_size as 3, and set the final output as 100 channels.
For the data adjustment part, I still set the normalization value as CIFAR100 mean and std.

## Pretrain Model Part 3
I also used ResNet50 and set pretrain as True.
### Performance
0.46948
### Adjustment
I set the normalization value to what ResNet model is using, since it was pretrained.

### Insight and Analysis
- The pretrained ResNet50 model acheived slightly better performance comparing to the non-pretrained version, which implies that transfer learning provides some improvement.  
- Using 128 batch size has a better performance than the optimal 512 batch size that was found by the batch size finder, which means we still have space of adjustment and need to try different parameters even we found an optimal one.
- The pretrained model showed signs of overfitting, as the training accuracy is around 99% while testing is about 55%, and the public accuracy is 46%. This implies that the model could be too complex for the CIFAR100 dataset.
- Data augmetation really helps in the process of training model. I tried different combinations of augmentations, including rotating and randomAffine, but found out that RandomHorizontalFlip and RandomCrop works better.


## AI disclosure
I uses github copilot to help me debugging and understanding the code structure. And I also uses deepSeek to help me understand what functions does Pytorch has and some explanation about different optimizer and scheduler. After using these AI tools, I tested different functions and optimizers and eventually defined the best model.

