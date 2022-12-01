# labelstudio-openshift-deploy

## Prerequisites

- Openshift cluster (Tested on 4.10)
- OC Command Line Utility
- Clone or download this repository

## Set Up Namespace and Install components

1. Set up core components
~~~
oc new-project labelstudio
  
oc apply -f postgres/
    
oc apply -f labelstudio/
~~~

2. Apply Open Data Hub Subscription and Apply Minio kfdef
~~~
oc apply -f odh-minio
~~~

## Create Data Bucket Using Minio

1. Fetch the Minio UI URL from routes in Openshift Under _Networking -> Routes_, or from:

~~~
oc get routes -n labelstudio
~~~

2. Access the UI from the "minio-ml-workshop-ui" route

![](docs/images/minio-console.png?raw=true "MinioConsole")

3. Log in to the console with:

~~~
user: minio
password: minio123
~~~

4. Under the _Buckets_ tab create one bucket for input data and one for labeled outputs:

~~~
labelstudio-src
labelstudio-results
~~~

![](docs/images/labelstudio-results-bucket.png?raw=true "LSBucket")

5. Create Bucket Access Key under _Access Keys_. Save the access key and secret or download them in a JSON file.

![](docs/images/accesskey.png?raw=true "accessKey")

## Configure Label Studio

1. Access the Label Studio route from the "label-studio-route" URL
2. Sign up with your email and a chosen password or re-use credentials
3. Click on "Create Project" and choose and a name for your project
4. Choose a template for your label setup (Ex: Text Classification) then click "Save"
4. In the project navigate to _settings -> Cloud Storage_ and configure storage for a data source (use the route URL for "minio-ml-workshop" and the access key created earlier):

![](docs/images/datasrc.png?raw=true "datasrc")


5.  Configure storage for a data target:

![](docs/images/datasink.png?raw=true "datasink")

6. Now Import Data to use for labeling (sample csv data is under /sample-data/nlp_sentiment.csv)

![](docs/images/data-screen.png?raw=true "data-screen")

7. Click on any item to add a label. The labeled results will be pushed to the minio results bucket as JSON objects.