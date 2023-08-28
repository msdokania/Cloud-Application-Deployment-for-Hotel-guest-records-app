# Cloud-Application-Deployment-for-Hotel-guest-records-app
# Use-case : Migration of On-premise hotel application to store guest Covid test results, to an Agile MultiCloud architecture, using Google Cloud for Virtual environment and AWS for storage of files.
(Credits - [https://thecloudbootcamp.com/]

In this project, Terraform is used for infrastructure provisioning : Provisioning of S3 bucket on AWS, and a Virtual network inside Google Cloud consisting of Cloud SQL and Kubernetes Engine. Database table was created to store records.

The application was deployed using a containerized architecture : The on -premise application was converted to a Docker image stored in Google Container Registry and the application was deployed using Kubernetes.

Data Migration : The on-premise data dump of customer records was imported to Cloud SQL, and the record pdf files were stored in the S3 bucket using AWS CLI.


*Detailed Steps :-*

1. Using IAM service under AWS Console, add a New User. Under Set permissions for the user, attach policies for Amazon S3 Full access
2. Go to Security Credentials tab and Create Access Key for the new user. Download the .csv file
3. Open Cloud Shell on Google Cloud platform (GCP) Console and upload the .csvfile with the access keys to AWS S3 instance as well as the Terraform infrastructure provisioning files
4. Move the .csv file into the __ folder and give permissions using chmod +x *.sh
5. Creates AWS credentials from an .csv file and given script using ./aws_set_credentials.sh <KEYS>.CSV
6. Set the Project ID in GCP environment and Add GCP project to configuration files by executing ./gcp_set_project.sh
7. Enable Container Registry API, Kubernetes API, Cloud SQL API by following commands -
gcloud services enable containerregistry.googleapis.com
gcloud services enable container.googleapis.com
gcloud services enable sqladmin.googleapis.com
8. Create the AWS S3 bucket and GCP Cloud SQL database from GCP Cloud shell itself using the Terraform files given by using -
terraform init
terraform apply
(Please Note : Before executing the .tf files update the bucket name as per your choice in tcb_aws_storage.tf)
9. Once the Cloud SQL instance is provisioned, select Default Associated networking, enable Service Networking API and use an automatically allocated IP range in your network, and create the connection.
10. Add a new Network. ONLY FOR TESTING PURPOSE, you can give public access to the network.


11. On Google Cloud platform (GCP) create a new user on newly created Cloud SQL MySQL database
12. Upload the --- files on Google Cloud Shell and connect to the MySQL DB running on Cloud SQL with the user created in above step.
13. Create a sample products table for testing purpose (you can use db/create_table.sql file)
14. Enable Cloud Build API using the command -
gcloud services enable cloudbuild.googleapis.com
15. Build the Docker image and push it to Google Container Registry using -
gcloud builds submit --tag gcr.io/<Project-ID>/<Name>
16. Kubernetes Deployment - Edit the .yaml file with correct variable values for Project ID, AWS bucket name, access keys, and Cloud SQL database Private IP. Connect to Google Kubernetes Engine Cluster and deploy the application in the Cluster using -
kubectl apply -f <filename>.yaml
17. Get the public IP and test the application!

18. Legacy Data Migration to AWS bucket -
Import a sample database dump (for testing purpose) to Google Cloud SQL, using the db_dump.sql file
On AWS Cloud Shell, execute - aws s3 sync . s3://<bucket_name>

