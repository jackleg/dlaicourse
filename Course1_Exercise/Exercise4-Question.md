
Below is code with a link to a happy or sad dataset which contains 80 images, 40 happy and 40 sad. 
Create a convolutional neural network that trains to 100% accuracy on these images,  which cancels training upon hitting training accuracy of >.999

Hint -- it will work best with 3 convolutional layers.


```python
import tensorflow as tf
import os
import zipfile
from os import path, getcwd, chdir

# DO NOT CHANGE THE LINE BELOW. If you are developing in a local
# environment, then grab happy-or-sad.zip from the Coursera Jupyter Notebook
# and place it inside a local folder and edit the path to that location
path = f"{getcwd()}/../tmp2/happy-or-sad.zip"

zip_ref = zipfile.ZipFile(path, 'r')
zip_ref.extractall("/tmp/h-or-s")
zip_ref.close()
```


```python
# GRADED FUNCTION: train_happy_sad_model
def train_happy_sad_model():
    # Please write your code only where you are indicated.
    # please do not remove # model fitting inline comments.

    DESIRED_ACCURACY = 0.999

    class myCallback(tf.keras.callbacks.Callback):
         # Your Code
        def on_epoch_end(self, epoch, logs={}):
            if(logs.get('acc') > DESIRED_ACCURACY):
                print("\nReached 99.9% accuracy so cancelling training!")
                self.model.stop_training = True

    callbacks = myCallback()
    
    # This Code Block should Define and Compile the Model. Please assume the images are 150 X 150 in your implementation.
    model = tf.keras.models.Sequential([
        # Your Code Here
        tf.keras.layers.Conv2D(16, (2, 2), activation='relu', input_shape=(150, 150, 3)),
        tf.keras.layers.MaxPooling2D(2, 2),
        tf.keras.layers.Conv2D(32, (2, 2), activation='relu'),
        tf.keras.layers.MaxPooling2D(2, 2),
        tf.keras.layers.Conv2D(64, (2, 2), activation='relu'),
        tf.keras.layers.MaxPooling2D(2, 2),
        tf.keras.layers.Flatten(),
        tf.keras.layers.Dense(512, activation='relu'),
        tf.keras.layers.Dense(1, activation='sigmoid')
    ])

    from tensorflow.keras.optimizers import RMSprop

    model.compile(loss='binary_crossentropy',
                  optimizer=RMSprop(lr=0.001),
                  metrics=['accuracy'])

    # This code block should create an instance of an ImageDataGenerator called train_datagen 
    # And a train_generator by calling train_datagen.flow_from_directory

    from tensorflow.keras.preprocessing.image import ImageDataGenerator

    train_datagen = ImageDataGenerator(rescale=1/255)

    # Please use a target_size of 150 X 150.
    train_generator = train_datagen.flow_from_directory(
                        '/tmp/h-or-s',
                        target_size=(150, 150),
                        class_mode='binary'
        )
    # Expected output: 'Found 80 images belonging to 2 classes'

    # This code block should call model.fit_generator and train for
    # a number of epochs.
    # model fitting
    history = model.fit_generator(
                        train_generator,
                        steps_per_epoch=8,
                        epochs=100,
                        verbose=1,
                        callbacks=[callbacks])

    # model fitting
    return history.history['acc'][-1]
```


```python
# The Expected output: "Reached 99.9% accuracy so cancelling training!""
train_happy_sad_model()
```

    Found 80 images belonging to 2 classes.
    Epoch 1/100
    8/8 [==============================] - 3s 375ms/step - loss: 1.1177 - acc: 0.6384
    Epoch 2/100
    8/8 [==============================] - 1s 99ms/step - loss: 0.3292 - acc: 0.8942
    Epoch 3/100
    8/8 [==============================] - 1s 91ms/step - loss: 0.1919 - acc: 0.9135
    Epoch 4/100
    8/8 [==============================] - 1s 100ms/step - loss: 0.0728 - acc: 0.9808
    Epoch 5/100
    6/8 [=====================>........] - ETA: 0s - loss: 0.0319 - acc: 1.0000
    Reached 99.9% accuracy so cancelling training!
    8/8 [==============================] - 1s 89ms/step - loss: 0.0259 - acc: 1.0000





    1.0




```python
# Now click the 'Submit Assignment' button above.
# Once that is complete, please run the following two cells to save your work and close the notebook
```


```javascript
%%javascript
<!-- Save the notebook -->
IPython.notebook.save_checkpoint();
```


    <IPython.core.display.Javascript object>



```javascript
%%javascript
IPython.notebook.session.delete();
window.onbeforeunload = null
setTimeout(function() { window.close(); }, 1000);
```


    <IPython.core.display.Javascript object>



```python

```
