# Serverless container on Google Cloud Run

## Clone the repository:
`git clone https://github.com/astro-derek/serverless-container.git`

## What does this code do?
* Remember we were saying serverless containers are good when we want multiple
processes in a container. This example does just that...
* When the container runs, it executes `start.sh` which in turn
* Starts an `nginx` instance to listen for web requests
* Starts a `Flask` python app to listen for requests
* The `nginx` configuration serves static files from the `static` folder
* any other requests are redirected to the python App
* so this is a small scale example but serves the purpose

## Build and deploy
The quickest and easiest way to create an image and deploy it is with the 
command: 

    cd serverless-container
    gcloud run deploy

You could also build the image separately too with this command:

    gcloud builds submit --tag gcr.io/${GOOGLE_CLOUD_PROJECT}/serverless:1.0.0 .

Then deploy with this command:

    gcloud run deploy --image=gcr.io/${GOOGLE_CLOUD_PROJECT}/serverless:1.0.0 --platform managed --concurrency 1

Note we're using concurrency 1 here... by default, Cloud Run will use concurrency
of 80 and it will be too hard for us to generate that many requests... so we'll
set it to 1 so each container handles a single request. This will make it easier
to see the auto scaling..


## Review Logs and Metrics
* The logs will show that the application has started successfully.
* They will show any errors that occur.
* We should be able to see the requests coming in.
* make several requests and review the logs and metrics
* It might be necessary to turn off caching in the browser to actually
make the container do extra work...
* Metrics should show us how many container instances are up and running.






# Example: multiple processes in a container (for Cloud Run)

Read the blog post for more details: http://ahmet.im/blog/cloud-run-multiple-processes-easy-way.

Build:

    docker build -t foo .

Run:

    docker run --port 5000:8080 --rm foo

Visit:

```sh
http://localhost:5000                  # served by Python server
http://localhost:5000/static/horse.jpg # served by NGINX
```

