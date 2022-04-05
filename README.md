# SCA-Cloud-School-Application
She Code Africa Cloud School Program- Technical Assesment


## EXERCISE 1:
- Create  a cloud account with a free tier subscription (AWS or AZURE).
 - Create an instance (either  a linux  or windows OS )	
 - Download and install Jenkins on your local machine to run on the default port 8080
 - Create a repository on Github called ``SCA Cloud School Application``
 - Write any simple code, in your  language of choice (c#, java,  python) on your local machine and push it to github .
 - Integrate  your github repo to your Jenkins 
 - Setup a  build job on Jenkins 
 - On submission , share the url to access your jenkin  server also advice   the   username and  password  for login (this can be different  from  your own account and you can also delete the account after the assessment )

## EXERCISE 2:
- Install docker on your machine 
- Create a docker hub account
- Create a Github repository called `SCA Cloud School Application`
- Create a Dockerfile which displays a webpage (in your preffered language ) and a text: ``Welcome to SCA Cloud School Application``
- Once done, run the container and test the application. Kindly Describe your test process and provide output

- Create a branch named ``Start`` and a folder named ``docker``
- Commit your Dockerfile and other files used in the ``docker`` folder
- Update the Dockerfile so the webpage displays ``Welcome to SCA Cloud School Application , this is my first assessment``
- Commit these changes to the repo into a branch called ``feature``and Merge your ``feature branch`` to the ``start branch`` ( _do not delete the feature branch_)
- Push your final docker image to dockerhub (https://hub.docker.com/)
- Your github repo ``Master`` branch should only have a readme file with instructions/documentaion used for your deployment and a link to your docker hub repository



Exercise2 ==>
### Step 1: Setup
Define the application dependencies. <br />
 1. Create a directory for the project:  <br />
`mkdir composetest`  <br />
 `cd composetest` <br />
 ![image](https://user-images.githubusercontent.com/78828566/161797124-583ff6ab-df66-4dde-b2b9-b2749074b31f.png)
 2. Create a file called `app.py` in your project directory and paste this in:
```
import time

import redis
from flask import Flask

app = Flask(__name__)
cache = redis.Redis(host='redis', port=6379)

def get_hit_count():
    retries = 5
    while True:
        try:
            return cache.incr('hits')
        except redis.exceptions.ConnectionError as exc:
            if retries == 0:
                raise exc
            retries -= 1
            time.sleep(0.5)

@app.route('/')
def hello():
    count = get_hit_count()
    return 'Welcome to SCA Cloud School Application! I have been seen {} times.\n'.format(count)
```
![image](https://user-images.githubusercontent.com/78828566/161814097-07f8420d-ac39-4826-afb2-a47a486ad169.png)

 3. Create another file called `requirements.txt` in your project directory and paste this in:
```
flask
redis
```
![image](https://user-images.githubusercontent.com/78828566/161814493-fb46b33b-edb8-4219-9041-0eb33b5f6192.png)

### Step 2: Create a Dockerfile
In this step, you write a Dockerfile that builds a Docker image. The image contains all the dependencies the Python application requires, including Python itself.

In your project directory, create a file named `Dockerfile` and paste the following: 
```
# syntax=docker/dockerfile:1
FROM python:3.7-alpine
WORKDIR /code
ENV FLASK_APP=app.py
ENV FLASK_RUN_HOST=0.0.0.0
RUN apk add --no-cache gcc musl-dev linux-headers
COPY requirements.txt requirements.txt
RUN pip install -r requirements.txt
EXPOSE 5000
COPY . .
CMD ["flask", "run"]
```
This tells Docker to:
* Build an image starting with the Python 3.7 image.
* Set the working directory to `/code`.
* Set environment variables used by the `flask` command.
* Install gcc and other dependencies
* Copy `requirements.txt` and install the Python dependencies.
* Add metadata to the image to describe that the container is listening on port 5000
* Copy the current directory `.` in the project to the workdir `.` in the image.
* Set the default command for the container to `flask run`.

### Step 3: Define services in a Compose file
Create a file called `docker-compose.yml` in your project directory and paste the following:
```
version: "3.9"
services:
  web:
    build: .
    ports:
      - "8000:5000"
  redis:
    image: "redis:alpine"
```

### Step 4: Build and run your app with Compose
1. From your project directory, start up your application by running `docker-compose up`.
![image](https://user-images.githubusercontent.com/78828566/161816274-4deb965a-447b-40d8-bcb4-15c347374abc.png)
![image](https://user-images.githubusercontent.com/78828566/161800321-22fe53e5-2b11-40cc-923e-59760516eb29.png)
Compose pulls a Redis image, builds an image for your code, and starts the services you defined. In this case, the code is statically copied into the image at build time.


2. Enter http://localhost:8000/ in a browser to see the application running. 
If this doesn’t resolve, you can also try http://127.0.0.1:8000.
You should see a message in your browser saying:
![image](https://user-images.githubusercontent.com/78828566/161811461-9cfe4480-cee2-4a2f-a545-58e353784ac1.png)

3. Refresh the page.
The number should increment.

4. Switch to another terminal window, and type `docker image ls` to list local images.
Listing images at this point should return `redis` and `web`.
![image](https://user-images.githubusercontent.com/78828566/161817246-a48ed78e-7767-4db3-9fc3-d9bd3005cfdf.png)

5. Stop the application, either by running `docker-compose down` from within your project directory in the second terminal, or by hitting CTRL+C in the original terminal where you started the app.


### Step 5: Experiment with some other commands
- If you want to run your services in the background, you can pass the `-d` flag (for “detached” mode) to `docker-compose up` and use `docker-compose ps` to see what is currently running.
- The `docker-compose run` command allows you to run one-off commands for your services. For example, to see what environment variables are available to the `web` service:
```
 docker-compose run web env
```
- If you started Compose with `docker-compose up -d`, stop your services once you’ve finished with them:
```
docker-compose stop
```
- You can bring everything down, removing the containers entirely, with the `down` command. Pass `--volumes` to also remove the data volume used by the Redis container:
```
docker-compose down --volumes
```

At this point, you have seen the basics of how Compose works.
FULL documentation [here](https://docs.docker.com/compose/gettingstarted/)

### Docker Build
1. Build with PATH
`docker build .`
![image](https://user-images.githubusercontent.com/78828566/161822290-1834ecf6-eb54-4a82-8be4-909b17b65e6a.png)


2. Build with URL
`docker build github.com/creack/docker-firefox`
![image](https://user-images.githubusercontent.com/78828566/161822559-383a22ad-b594-4d66-82e0-169da1ff10f6.png)


3. Build with -
`docker build - < Dockerfile`
This will read a Dockerfile from `STDIN` without context. Due to the lack of a context, no contents of any local directory will be sent to the Docker daemon. Since there is no context, a Dockerfile `ADD` only works if it refers to a remote URL.
`docker build - < context.tar.gz`
This will build an image for a compressed context read from `STDIN`. Supported formats are: bzip2, gzip and xz. 

FULL documentation [here](https://docs.docker.com/engine/reference/commandline/build/)

``
