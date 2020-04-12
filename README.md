# Improving CNN performance with iterative steps on the CIFAR10 data set


## Approach

### Initial Network

- Build CNN Model using Keras
- Full Connected output layer
- Parameters 1.18 MM
- the base accuracy of above model was 82.04%

>### Observations :

>- Validation accuracy did not increase too much (plateaus)after 20 epochs
>- Validation loss dropped the most till 20 epochs and then increased gradually
>- Input image in 32*32
>- But but the time convolutions are done - we are at 64*64 please see the comments
>- Dropouts close to prediction is 50% - which is too high (might drop out required features)

### Approach and Observations

- Removed the Fully onnected output layer and replaced with Convolutions
- Parameters 1.18 MM


- **First model no dropout; Batch Normalization. 20 epochs**
>- Started to overfit after 5 epochs. Validation accuracy was at .73

- **Added BN and Dropout(0.1)**
>- The validation accuracy increased to 0.79 , but was not smooth. The validation accuracy was in upward trend - so definetely potential to improve. Increased batch size to 256

- **The Receptive Field was at 24*24 last convolution layer. So potentially not learning from full image**
>- One solution is to add more layers, but the size of the image in last layer will drop less than 6*6 - which will impact prediction. Increase Receptive Field from 24 to 32 will need 4 more layers. This will impact image size
Using Border mode

- **Added 2 layers with border_mode = same**.This increased the Receptive Field to 32*32 without reducing the image size. Potentially adding a bit of noise. But, this might have an effect of regularization and help with overfitting.Not adding dropout after using border_mode = 'same' as might have a duplicate effect.600K parameters .Validation accuracy was .8106
>- Realized adding Border mode = 'same' will actually add more than 10% of dummy pixels. Hence a dropout of 0.1 may not be enough. Increased the dropout to 0.25 after adding border_mode = 'same'. Also added the custom L2 regularization. Redcued the batch size to 128 .Validation accuruacy was 0.7975 after 20 epcohs

- **Enabled horizontal flip to True**. 
>- This is a form of data augmentation ( A cat looking right and a cat looking left is still a cat ).After 20 epochs was close to .8220. Hence adding the horizonatal flip definitely helped.

## Run with 100 epochs.Validation accuracy of 84.65%.

