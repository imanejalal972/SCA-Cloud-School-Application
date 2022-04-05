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

![image](https://user-images.githubusercontent.com/78828566/161797124-583ff6ab-df66-4dde-b2b9-b2749074b31f.png)
![image](https://user-images.githubusercontent.com/78828566/161800321-22fe53e5-2b11-40cc-923e-59760516eb29.png)
![image](https://user-images.githubusercontent.com/78828566/161811461-9cfe4480-cee2-4a2f-a545-58e353784ac1.png)
