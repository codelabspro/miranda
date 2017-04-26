# miranda

## Steps as described here 

https://cloud.google.com/endpoints/docs/frameworks/python/quickstart-frameworks-python


## Enable virtualenv and install requirements
virtualenv venv
source venv/bin/activate
cd miranda
pip install -r requirements.txt


## Steps

### Update the Cloud SDK and install the Endpoints components:
==> gcloud components update

### Make sure that Cloud SDK (gcloud) is authorized to access your data and services on Google Cloud Platform:
==> gcloud auth login


### Set the default project to your project ID:
==> gcloud config set project [YOUR-PROJECT-ID]


### Install the SDK for App Engine:
==> gcloud components install app-engine-python


Adding the Cloud Endpoints Frameworks library to the sample API

To add the frameworks library to your app so it will be uploaded with it upon deployment:

Make sure you are in the sample API's main directory, /python-docs-samples/appengine/standard/endpoints-frameworks-v2/echo.
Make a /lib subdirectory in the project:

mkdir lib
From the sample main directory /python-docs-samples/appengine/standard/endpoints-frameworks-v2/echo, invoke the following install command:

pip install -t lib google-endpoints --extra-index-url=https://gapi-pypi.appspot.com/admin/nurpc-dev --ignore-installed
Notice that this library is vendored in the file /python-docs-samples/appengine/standard/endpoints-frameworks-v2/echo/appengine_config.py


Generating the OpenAPI configuration file

To generate the required configuration file:

Make sure you are in the sample main directory /python-docs-samples/appengine/standard/endpoints-frameworks-v2/echo,
In the sample main directory /python-docs-samples/appengine/standard/endpoints-frameworks-v2/echo/, in Python, invoke the Endpoints Tool to generate the OpenAPI spec:

python lib/endpoints/endpointscfg.py get_openapi_spec main.EchoApi --hostname echo-api.endpoints.[PROJECT-ID].cloud.goog
Replace [PROJECT-ID] with your project ID. Do not include the square brackets. Ignore the warnings that are displayed.


Deploying the OpenAPI configuration file

To deploy the configuration file:

Deploy the configuration file by invoking the following command:
gcloud service-management deploy echov1openapi.json
Display the service configuration ID by running the following command:
gcloud service-management configs list --service=echo-api.endpoints.[PROJECT-ID].cloud.goog
Replace [PROJECT-ID] with your project ID. Do not include the square brackets.
Edit the file python-docs-samples/endpoints-frameworks-v2/echo/app.yaml, and make the following changes in the env_variables section:
env_variables:
  # Replace with your endpoints service name.
  ENDPOINTS_SERVICE_NAME:
  echo-api.endpoints.[PROJECT-ID].cloud.goog
  # Replace with the version Id of your uploaded Endpoints service.
  ENDPOINTS_SERVICE_VERSION: [YOUR-SERVICE-CONFIG-ID]
  
Replace [PROJECT-ID] with your project ID and [YOUR-SERVICE-CONFIG-ID] with your service configuration ID from the previous step. Do not include the square brackets.


Deploying the sample API

To deploy the sample API:

Invoke the command:

gcloud app deploy
Wait a few moments for the deployment to succeed, ignoring the warning messages. When the deployment completes, you'll see a message similar to the following:

Deployed service [default] to [http://miranda-165804.appspot.com]

You can stream logs from the command line by running:
  $ gcloud app logs tail -s default

To view your application in the web browser run:
  $ gcloud app browse

File upload done.
Updating service [default]...done.
Sending a request to the sample API

After you deploy the API and its configuration file, you can send requests to the API.

To send a request to the API:

Run the following curl command:

curl -H "Content-Type: application/json" -X POST -d '{"content":"Hello world!"}' https://miranda-165804.appspot.com/_ah/api/echo/v1/echo
Replace [PROJECT-ID] with your project ID. Do not include the square brackets.
View the results: you should see:

{
 "content": "Hello world!"
}








