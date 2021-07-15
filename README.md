# Two-Class-Boosted-Decision-Tree-for-Bank-Telemarketing-Prediction---using-Microsoft-AzureML-Studio

- The data here is related with direct marketing campaigns of a Portuguese banking institution. The marketing
campaigns were based on phone calls and often more than 1 contact to the same client was required.
This was done to find out if the product would be subscribed or not. `The classification
goal is to predict if the client will subscribe for a term deposit.`
- This is a supervised learning problem and it is of type two-class classification.
There are various features of this dataset and we will go to the details when we visualize the dataset
in Azure ML. You can also find this dataset
from the website of UCI and it is publicly available. You can search for it as bank marketing dataset
of UCI and it should provide you among the first 3 links.
- I would suggest read about [Boosted Decision Tree](https://docs.microsoft.com/en-us/azure/machine-learning/algorithm-module-reference/boosted-decision-tree-regression) before moving forward.
- Sign in to your Microsoft azure account. ( you can make a free trial account for a month [here](https://azure.microsoft.com/en-in/free/search/?&ef_id=CjwKCAjwlrqHBhByEiwAnLmYUA_xQpYpL-GyL0_xeq6PiMMIEJoCUSsH0t9AEomzarx65DD4luwkcBoCEUoQAvD_BwE:G:s&OCID=AID2200195_SEM_CjwKCAjwlrqHBhByEiwAnLmYUA_xQpYpL-GyL0_xeq6PiMMIEJoCUSsH0t9AEomzarx65DD4luwkcBoCEUoQAvD_BwE:G:s&gclid=CjwKCAjwlrqHBhByEiwAnLmYUA_xQpYpL-GyL0_xeq6PiMMIEJoCUSsH0t9AEomzarx65DD4luwkcBoCEUoQAvD_BwE) but will also have to add your credit card details)
- We will use the [Microsoft Azure Machine learning studio](https://studio.azureml.net) which is great for beginners.

## 1)Visualising the dataset : 
   - Upload the dataset in Azure ML Studio by selecting the `NEW` option.
   - Once you have uploaded the dataset, create a new `Blank Experiment` after selecting the `NEW` option again.
   - Drag the dataset in the working space and visualise it : 
   - <img width="600" alt="Screen Shot 2021-07-15 at 8 39 17 PM" src="https://user-images.githubusercontent.com/55271617/125811686-a4a84708-4180-4563-a619-4e947027111c.png">
   - <img width="600" alt="Screen Shot 2021-07-15 at 8 40 46 PM" src="https://user-images.githubusercontent.com/55271617/125811910-18139ba8-6db5-4c97-841b-991687e05f72.png">
   - Try to vusialise every column, you may see we don't have any missing values. For eg. second feature is job which is a string feature and has 12 unique values such as management, technician
blue-collar and so on.
  - We do not have any missing values, so we are going to straightaway jump to the split module.

## 2)Splitting Data :
We will split the existing data and use part of the data for training our model and the rest for testing the result as well as validating the results for accuracy.
- Drag and drop the `split data` module.
- We will split the data in 70:30 ratio and lets do the [stratified split](https://stats.stackexchange.com/questions/250273/benefits-of-stratified-vs-random-sampling-for-generating-training-data-in-classi) on the column, `Y` so that we have even distribution of values between train and test dataset.
- <img width="600" alt="Screen Shot 2021-07-15 at 8 49 39 PM" src="https://user-images.githubusercontent.com/55271617/125813389-329885f3-aa15-4715-924b-c53241229452.png">
- Now Run the `split data` module and visulaise the two output nodes.(one has 70 percent data and one 30 percent)

## 3)Training and scoring our two-class Decision Tree Model :
We are going to predict an outcome that's either yes or no, which means that we are only predicting two classes, so we will use two-class boosted Decision Tree Model.
- Drag and drop the `Two- Class Boosted Decision Tree` Model, you can also change the parameters and visualise the change in results with different values of parameters.
- <img width="600" alt="Screen Shot 2021-07-15 at 8 54 34 PM" src="https://user-images.githubusercontent.com/55271617/125814268-b98b7ed6-1f8f-4026-9c71-9eb26dca1939.png">
- Lets talk about different parameters :
    - Maximum number of leaves we want per tree :  leaves or terminal nodes are the nodes which we can not or do not want to split further. By increasing this value, you are potentially increasing the size of the tree and getting better precision. The better precision here, is at the risk of overfitting and longer training time.
    - Minimum number of samples per leaf node : indicates the number of cases or observations required to create any terminal node or leaf in a tree. By increasing this value, you increase the threshold for creating new rules. For example, with the value of Y, the training data will have to contain at least 5 cases that meet the same conditions before we can split it further.
    - The learning rate determines how fast or slow the learner conferges on the optimal solution. If the step size is too big, you might overshoot the optimal solution. And if the step size is too small, training may take longer to arrive at the best solution.
    - Number of trees constructed indicate the total number of decision trees to create. By creating more decision trees you can potentially get better coverage, but the training time will increase.
- For training our model, drag and drop the `train model` module (selecting Y as the column selector) and connect them as shown below.
- <img width="600" alt="Screen Shot 2021-07-15 at 9 09 32 PM" src="https://user-images.githubusercontent.com/55271617/125816439-a27465c1-01aa-47aa-8bff-dab952146787.png">

- The model is ready to be trained, we want to score the algorithm on our test dataset and see how it performs. So the module that is required for the same is called as a score model, which is basically a validation module for our trained algorithm. So let's search for it and drag and drop it hereand as you will see, it requires 2 inputs. The first one is the train model which is nothing but the output from the train model module and the test dataset.
- So we provide the connection of our train model as well as the test dataset. The test dataset is coming from the second node of split module. Now, let's right click on it and run selected. It will run all the previous steps that have not been executed so far.
- <img width="600" alt="Screen Shot 2021-07-15 at 9 11 22 PM" src="https://user-images.githubusercontent.com/55271617/125816708-2df38a8d-d7b1-405a-b592-c50927baf519.png">
- Now we can visualise the output.
- <img width="600" alt="Screen Shot 2021-07-15 at 9 12 57 PM" src="https://user-images.githubusercontent.com/55271617/125816945-c4056449-dfd8-451f-95f2-fa4ea43920bd.png">
- As you can see, there are two additional columns called scored label and score probabilities, scored label is the predicted value of that particular set of features or rows and score probability is the probability with which this particular value has been predicted.

## 4)Evaluating our model :
Lets visualize these results in a bit more structured manner.
- We will use a module called `evaluate model` for evaluating our results.
- It takes 2 nodes which are for evaluating different models. The second one is optional and let's connect our scored model to the first node and run it.
- <img width="600" alt="Screen Shot 2021-07-15 at 9 14 25 PM" src="https://user-images.githubusercontent.com/55271617/125817175-34cbee29-9649-4ae5-ac9f-29b98ba9e1ff.png">

- After visulising the output, we can say the accuracy is around `91%` and the AUC i.e area under the curve is also very high which means that the model is a very good model.
- <img width="600" alt="Screen Shot 2021-07-15 at 9 16 15 PM" src="https://user-images.githubusercontent.com/55271617/125817433-cac7ef30-b9ce-4342-ab71-bdd85256ea5a.png">


## Thank You, Hope the explanation was clear and to the point !
