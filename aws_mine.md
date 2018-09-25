Instructions for setting it up:
- Download Iannis aws repo: https://github.com/uizard-io/awszard-switch-role
- Install it as: `python setup.py install`
- `touch ~/.aws/config`
- Write  [profile uizard_v1] output = json
- `touch  ~/.aws/credentials`
- [uizard_v1] aws_access_key_id =  aws_secret_access_key = 
You get the keys when logging in the account IAM. The secret comes in a csv fiel that you download when creating the key

Now for logging:
-`eval $(awszard-clear-role)`
-`eval $(awszard-switch-role --profile uizard_v1 --role play_trainer_v1 -t <mfa-key>)` 


*AWS cheatsheet*:
- Copying folder on instance: `scp -r labs-denoising-autoencoder/ ec2-user@54.225.6.83:/home/ec2-user`
- Getting credentials: `eval $(awszard-switch-role --profile uizard_v1 --role play_trainer_v1 -t 695003)`
- Connecting thru ssh: `ssh ec2-user@54.225.6.83`
- Copying from s3 to instance: `aws s3 sync s3://io.uizard.play.training-data.eu-central-1/autoencoder/data/v2 ./data`
- Copying to s3(weights): `aws s3 cp ./my_awesome_weights_v100.h5 s3://io.uizard.play.training-data.eu-central-1/autoencoder/weights/`

![alt text](https://github.com/artuntun/nerdy_resources/blob/master/images/aws_logging.png"Logo Title Text 1")
