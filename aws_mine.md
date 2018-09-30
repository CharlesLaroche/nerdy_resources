<script type="text/x-mathjax-config">
MathJax.Hub.Config({
tex2jax: {
inlineMath: [['$','$'], ['\\(','\\)']],
processEscapes: true},
jax: ["input/TeX","input/MathML","input/AsciiMath","output/CommonHTML"],
extensions: ["tex2jax.js","mml2jax.js","asciimath2jax.js","MathMenu.js","MathZoom.js","AssistiveMML.js", "[Contrib]/a11y/accessibility-menu.js"],
TeX: {
extensions: ["AMSmath.js","AMSsymbols.js","noErrors.js","noUndefined.js"],
equationNumbers: {
autoNumber: "AMS"
}
}
});
</script>
$$F = m_26$$
Basic instances workflow:

1. Log in AWS and change the region to N.West(Virginia)
2. Change the role to:
  - Account: `aws-play-uizard-io`
  - Function: `Trainer`
3. Trhow an instance: **Deep Learning AMI (Amazon Linux)**
4. Log in in the terminal with:
  - Clear:  `eval $(awszard-clear-role)`
  - Own credentials: `eval $(awszard-switch-role --profile uizard_v1 --role play_trainer_v1 -t <mfa-key>)`
5. Connect to the instance: `ssh ec2-user@54.225.6.83`
6. Copy your own local project: `scp -r src/ ec2-user@52.90.178.152:/home/ec2-user/labs-UI-segmentation`
7. Copy data from s3(within instance): `aws s3 cp s3://io.uizard.play.training-data.eu-central-1/components/sep_18/components.zip ./data/`

================== AFTER TRAINING ============

7. Copy outputs to s3(within instance): `aws s3 cp weights2 s3://io.uizard.play.training-data.eu-central-1/segmentation-ui/weights/`
8. Copy file from s3 locally: `aws s3 cp s3://io.uizard.play.training-data.eu-central-1/segmentation-ui/weights/weights2.py path/locally`
9. Copy folder from s3 locally: `aws s3 sync s3://io.uizard.play.training-data.eu-central-1/segmentation-ui/weights/ path/locally`



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

![alt text](https://github.com/artuntun/nerdy_resources/blob/master/images/aws_logging.png "Logo Title Text 1")
