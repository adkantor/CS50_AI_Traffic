# The experimentation process

## Step 1: build a naive benchmark model

As a naive benchmark model I've chosen Logistic Regression used as multiclass estimator, with input data reshaped to 1D and one-hot encoded labels.

Validation of the model performance was made using *10-fold Cross-Validation*.

### Results
- **Small dataset**: Accuracy: 0.999 with Standard Deviation: 0.004
- **Large dataset**: Accuracy: 0.926 with Standard Deviation: 0.005

>**Note**: for details see *traffic_notebook_benchmark.ipynb* or *traffic_notebook_benchmark.html*

---

## Step 2: build simple Sequential model (without convolution) as benchmark

To explore effect of different layer sizes and number of layers I built some models without convolution, using the **Small dataset**. Models were trained for 20 epochs.

Evaluation was made on a 20% hold-out test set.

### Results

- **1 hidden layer of 8 units**

    loss=0.0072, accuracy=100.000%

    Both train and test accuracy plateaued after epoch 12 while loss showed minimal decrease even in the last epochs.

- **1 hidden layer of 128 units**

    loss=0.0041, accuracy=100.000%

    Both train and test accuracy plateaued after epoch 12 while loss showed minimal decrease even in the last epochs.

- **3 hidden layers of 128 units**

    loss=0.0005, accuracy=100.000%

    Train accuracy plateaued after epoch 4 at 100% while test accuracy plateaued after epoch 2 at 99.26%. Loss remained unchanged after epoch 10.

I've chosen to proceed with **1 hidden layers of 128 units**.

## Step 3: add convolutional layers (still on Small dataset)

I added a single convolutional layer with 32 filters using 3x3 kernel.

Decreased number of epochs to 10.

### Results

    loss=0.0009, accuracy=100.000%

    Both train and test accuracy plateaued after epoch 4. Loss remained unchanged after epoch 6.

---

## Step 4: Try model of step 3 on Large dataset.

### Results

    loss=0.1931, accuracy=95.852%

    Both train and test accuracy plateaued after epoch 4. Loss remained unchanged after epoch 6.

    Model seems to be slightly ***overfitting*** as validation accuracy plateaued around 95% while train accuracy still showed increasing accuracy during the last epochs.

    The training history also showed significant variance.

---

## Step 5: Explore different configurations

- **Add 25% dropout to the hidden layer, increase number of epochs to 20**

    loss=0.1661, accuracy=96.809%

    Validation accuracy plateaued after epoch 14 at 97% while train accuracy still showed increasing accuracy during the last epochs. Loss remained unchanged after epoch 5 with significant variance.

- **Increase dropout to 50%**

    loss=0.1248, accuracy=96.988%

    Both train/test accuracy and loss showed improvement in the last epochs, so number of epochs needs to be increased.

- **3 hidden layers with 256/128/64 units, increse number of epochs to 40, add early stopping**

    loss=0.1673, accuracy=96.453%

    Seamingly performance has decreased however, history plot shows room for further improvement if number of epochs are increased (performance has not plateaued, early-stopping has not occured).

- **3 convolutional layers with 32 filters using 3x3 kernel, 3 hidden layers 128 units, increse number of epochs to 50**

    loss=0.0576, accuracy=98.649%

    Performance measures show significant improvement after adding more convolutional layers. Early stopping occured after epoch 37.

I decided to use this last model as the Final Model.

>**Note**: for details see *traffic_notebook_models.ipynb* or *traffic_notebook_models.html*