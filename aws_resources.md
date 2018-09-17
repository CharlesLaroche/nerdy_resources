# Useful notes on AWS Tour

**Date:** 2018/06/20

**Author:** Alexandre Zouaoui (alexandre@uizard.io)


On this day we did a quick tour of AWS with Ioannis and Tony helped launch the instance.

## Account and registration

First Ioannis created an account for me so that I could launch log in my AWS account [here](https://us-east-1.signin.aws.amazon.com/oauth?SignatureVersion=4&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAJMOATPLHVSJ563XQ&X-Amz-Date=2018-06-20T12%3A12%3A26.409Z&X-Amz-Signature=b5c2ae870284ef5d8c63ce7623b38098ca439d4c7dc9ca8869921ff4424fdab1&X-Amz-SignedHeaders=host&client_id=arn%3Aaws%3Aiam%3A%3A015428540659%3Auser%2Fhomepage&redirect_uri=https%3A%2F%2Fconsole.aws.amazon.com%2Fconsole%2Fhome%3Fstate%3DhashArgs%2523%26isauthcode%3Dtrue&response_type=code&state=hashArgs%23).

Username: alexandre.zouaoui ``<first_name>.<last_name>``

Password: Password policy requires at least a 12 letters long password with at least 1 uppercase letter, 1 lowercase, 1 special character and 1 number. You also need a password manager and do not save it in your browser.

We have also set up a 2-steps factor authentication.

Whenever I log in, I end up in the **root** account where there is nothing except the ability to manage the user access. From there we jump on a specific account/role.

I will be using the **play** account having a **trainer** role.

## Access from a terminal

Having downloaded a private [repository](git@github.com:uizard-io/awszard-switch-role.git) made by Ioannis, I can use it by typing ``eval $(awszard-switch-role --profile uizard_v1 --role play_trainer_v1 -t <2sFA code>)``. The profile can be found under the ``~/.aws/configure`` configuration file while the namespace for the role is based on ``<account>_<role>_<version>``.
This basically creates a temporary token or cookie that can be used to access the AWS services from a local terminal.

I needed to download the ``awscli`` Python package on my global system Python.

## Services

There are 2 services that I will need to use extensively, namely ``S3`` and ``EC2``.

### S3

**S3** is a storage service. Based on my current account/role I can only access and modify the ``io.uizard.play.training-data.eu-central-1`` bucket.

There is a bunch of commands that can be used to download and upload data on s3, namely:
* ``aws s3 sync s3://<bucket>/<folder_path>/ <local_folder_path>`` copies an entire directory to the destination path.
* ``aws s3 cp s3://<bucket>/<file_path> <local_file_path>`` copies a file to the destination path

You can upload data by switching the input and destination paths.

### EC2

**EC2** is a Cloud Computing service. Based on my current account/role I can use it to launch a Amazon Machine Image (AMI) to train a model. I typically favor the **Deep Learning AMI (Amazon Linux)** instance because it comes prepared with conda environment such as a TensorFlow(+Keras) with Python3 that you can simply activate by running ``source activate tensorflow_py36``.

* Launching an instance is pretty straightforward:
  1. Choose an Amazon Machine Image (AMI).
  2. Choose an instance type (p3.2xlarge is good enough).
  3. Configure the instance to select the pre-made IAM group.
  4. Add Storage (keep previous value if it's already there).
  5. Add a **Name** tag to differenciate your instance with the other (e.g. ``alex-training-1``)
  6. Configure the security group (leave it has is)
  7. Review and launch

* To log into the machine, simply use ``ssh ec2-user@<dns>``

* To import/export file, simply use ``scp <local_file_path> ec2-user@<dns>:<remote_file_path>``. Add ``-r`` if you wish to copy a folder.

* You may have to download some package using pip inside the virtual environnment (I had to install OpenCV2).

* You can even launch a notebook to try out some tutorials.

* If you are training using a Python script, there is a handy command to let it run regardless of the state of your session. You can basically let it run while your computer is sleeping.
``nohup ./train.py -0 ../data/random/training_set/ -1 ../data/mockups/training_set/ -2 ../data/sub_drawings/training_set/ -3 ../data/sketched_ui/processed_data/training_set/ -s 80 -e 100  > ../model/multiclass_training_output.txt 2>&1 </dev/null &``
Basically the ``nohup`` command create a background thread with the input script and its arguments. You also need to add an output file that you can ``cat | grep`` from time to time to check that you are still training.

* When you are done training a model, do not forget to upload the weights of the best checkpoint on s3.

* You can even zip the training/evaluation set in order to upload them on s3 for later use.

* You can exit the remote ssh control by entering ``exit``.

* **PLEASE REMEMBER TO TERMINATE THE INSTANCE WHEN YOU ARE DONE**. This can be done by going to the Instances Tab on the AWS platform. Select the machine you wish to terminate. Click on the Actions button and do:
  1. Actions
  2. Instance State
  3. Terminate
