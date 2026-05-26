-------- Append the code under "terraform" block with your backend for storing the state file at the S3 bucket. --------


terraform {
  backend "s3" {
    bucket         = "THE_NAME_OF_THE_STATE_BUCKET"
    key            = "some_environment/terraform.tfstate"
    region         = "us-east-1"
    encrypt        = true
    kms_key_id     = "THE_ID_OF_THE_KMS_KEY"
    dynamodb_table = "THE_ID_OF_THE_DYNAMODB_TABLE"
  }
}



------------------------------ While adding to backend add at the top & under "terraform" ------------------------------
