
#sagemaker


---

link to the course: [link](https://www.youtube.com/watch?v=Ld2oTLY47sA)

Why do we use Sagemaker?
	1. Model training at scale.
	2. Model deployment and inference.
	3. Data labeling and preparation.
	4. Experiment tracking and Hyper-parameter tuning.
	5. MLOPs and pipelines, CI/CD for ML.
# 0. Buzz words.





# 0.1 Setting up AWS account


Two types of users:
1. root user:  has access to everything
2. IAMUSER : does not have access to everything, *(recommended to use)*


+ Setting up IAMUSER:
	+ we need to give different types of access to  the account  currently we are giving adminstratoraccess.
	+ 



# 0.2 AWS security best practices


+ Make sure to be closer to the region that is shown.

+ How  to delete the user?
+ etc



# 1. Set up AWS sagemaker Domain

To set up sagemaker we need to create a domain first.


DOMAIN:
	env where the team has the access to the Sagemaker resources.


+ Creating a domain.
+ inside domain create a user.
	+ Here rwe enable the access to jupyterlab, code editor, canva ete.
+ Using jupyter lab.
	+ here we can select which kind of compute power we need.
	+ this as all the libraries already installed on it.
	+ we can also give the remote access  here to our jupyter environment  but we need to select the vcpu with memory atleast 8gb.

link = [link to sagemaker studio](https://studio-d-9grqnmj5y1kb.studio.eu-central-1.sagemaker.aws/home?tab=Overview)




# 1.1 Sagemaker studio and pricing


look for pricing here: [link](https://aws.amazon.com/sagemaker/ai/pricing/)


click the accelerated instances to look for GPU:
	which every instance name has "P" in it, that is a gpu.

IMPORTANT:
	+ Always turn off the notebook and other things as it cost money.
	+ imgaes does not cost any price. (recommended use: pytorch image with python 10)

# 1.2 Sagemaker servercies and pytorch


IMPORTANT:
	+ do not store the data on EFS i.e the usual place where we store the files in jupyter notebook (left side bar).
	+ always save the code and turn off the running apps.


# 1.3 Hugging face models sentiment analysis and autoscaling



AUTOSCALING:
	+ we can set up the rules where we define the resources for the model
	+ in the endpoints we can set up the cpu utilization and other things

IMPORTANT:
	+ Delete the endpoints after using them otherwise it will cost money.



# 2. Getting dataset for multiclass text classification.


Uploading the dataset to S3 bucket:
+ search for s3 bucket in the aws dashboard
+ create a bucket and upload the data into it.
+ after uploading we can copy the path of the csv from the bucket and the use it in our sagemaker studio.


- [ ] Not able to open the data.



---
 [[AWS WITH IOT CORE]]  