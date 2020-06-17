![1 TrK7a2kW0Ut1CwjyJ5rWvg](https://user-images.githubusercontent.com/66811679/84464572-bcd60280-ac29-11ea-9dff-04a0a922d390.png)

# Objective :

Setup up an infrastructure such as there are three teams/environemnts :

1. Production
2. Testing
3. Quality Assurance

Production Team will deploy the code first, now there is some work going on in the developement team which is to be deployed on testing environment which will be sent to the production only when the QA team approves it.

## Requirements :

1. Docker
   - Image: httpd
2. Jenkins
   - Github Plugin
3. System with 2gb ram (Recommended)
4. Git and Hooks
5. Github and Github Webhooks
6. ngrok

# My Approach

1. Made 3 Jobs for produnction, testing and Quality Assurance

2. Make Environments for production and testing. I am using docker here, I have created a volume(persistant storage) for production and testing and mounted them to their respective containers

## Process :

### Operations Side:

1. My job 1 and job2 will get their code from the github (from their respective branches => master:production, dev:testing)
2. Both the containers have diferent web addresses so the quality team can monitor both
3. Quality team continously checks for changes in testing environemnt, and decides wether to approves it or not
4. On approval QA team manually builds the job3
5. Since job three is the upstream job of job2 and job1 , so on succesfull approval foloowong things will happen:
   1. Dev and Master branch will be merged
   2. Code will be pushed to the github
   3. job 1 and job2 will be build

- ngrok will be running in the system to provide public accessable addres so that github webhook will work and production site can be accessible to the public

### Developer site :

1. Created a hook for post-commit and post-merge so the developer need not to be manually push the code each time he commits
![post-commit](https://user-images.githubusercontent.com/66811679/84467167-222cf200-ac30-11ea-96c8-ab2b7be0d554.png)

![2](https://user-images.githubusercontent.com/66811679/84903066-7a198d80-b062-11ea-8b7c-63ba0cd8b051.PNG)



2.Added The webhooks to the Jenkins Github API :=> ip/github-webhook/
![webhooks](https://user-images.githubusercontent.com/64473684/84473373-94d8b480-aca6-11ea-82bf-ecb0e698d38c.jpg)
## Screenshots :

## 1. Initially:

1. Production:

![8082](https://user-images.githubusercontent.com/66811679/84866656-d8c51400-b02e-11ea-9f7e-2a009a5e9b8c.PNG)


2. Testing :

![8083](https://user-images.githubusercontent.com/66811679/84866827-1e81dc80-b02f-11ea-822b-de0aa946f7aa.PNG)
## 2. Changed made but not approved:

1. Produnction:

![8082](https://user-images.githubusercontent.com/66811679/84866656-d8c51400-b02e-11ea-9f7e-2a009a5e9b8c.PNG)

2. Testing :

![8083](https://user-images.githubusercontent.com/66811679/84866827-1e81dc80-b02f-11ea-822b-de0aa946f7aa.PNG)


3. QA Rejected

## Changed and approved:

1. Testing :

![poi](https://user-images.githubusercontent.com/66811679/84868004-db286d80-b030-11ea-8102-bab10ee030a9.PNG)

2. QA Approved:


![kl](https://user-images.githubusercontent.com/66811679/84886506-601f8100-b049-11ea-9db1-bcc28922ca4d.PNG)


![qa1](https://user-images.githubusercontent.com/66811679/84495389-f295de00-ac5f-11ea-9d17-47b6f834af84.jpg)

![kk](https://user-images.githubusercontent.com/66811679/84880754-d0c29f80-b041-11ea-8178-1bf0c7ab5896.PNG)

3. Production:

![poi](https://user-images.githubusercontent.com/66811679/84868004-db286d80-b030-11ea-8102-bab10ee030a9.PNG)


## Steps and Configurations Screenshots :

## 1. Production:

* Created a job for cloning the Github repo's master branch which is to be deployed on docker.
Github Githook trigger is checked so that whenever the developer pushes the code, github webhook will be activated and it will    contact jenkins, then jenkins will pull the updated code, and move it to the created volume.


![44](https://user-images.githubusercontent.com/66811679/84869621-204d9f00-b033-11ea-8b56-8bdc8bb34e68.PNG)

* Provided the command to check wether the docker conatiner is already running, if not create it mount the volume and expose it's port 80 to base os port 8081.


![oo](https://user-images.githubusercontent.com/66811679/84498431-81592980-ac65-11ea-824a-adc30a01bd24.PNG)


## 2. Testing:

* Created a job for cloning the Github repo's master branch which is to be deployed on docker.
Github Githook trigger is checked so that whenever the developer pushes the code, github webhook will be activated and it will contact jenkins, then jenkins will pull the updated code, and move it to the created volume.



![Capture](https://user-images.githubusercontent.com/66811679/84872225-753ee480-b036-11ea-8446-a45fedb7881e.PNG)

* Provided the command to check wether the docker conatiner is already running, if not create it mount the volume and expose it's port 80 to base os port 8082


![1](https://user-images.githubusercontent.com/66811679/84901700-b4822b00-b060-11ea-8290-40a2b81b0381.PNG)



## 2. QA Team :

* Provided the github repo link along with credentials as on approval there should be a merge between the master and dev branch, and also the code should be pushed to github.

![po](https://user-images.githubusercontent.com/66811679/84875567-f6987600-b03a-11ea-9d5b-132af60f9af2.PNG)


* Details is provided to the additional behaviour so that merging could be done.

![xp](https://user-images.githubusercontent.com/66811679/84506637-8376b480-ac74-11ea-856a-5f5959796cd6.PNG)

* Post Build Action deatils provided, so that push after the merge can be done

![xp1](https://user-images.githubusercontent.com/66811679/84506985-fed86600-ac74-11ea-951a-3211db29a491.PNG)

* After the push is being done. This job will call the job1 and job2 so that the code can be updated. (This part can be omitted as webhook will do it's job, but to be on safe side, I have put these projects as downstream projects)


![xp2](https://user-images.githubusercontent.com/66811679/84506975-f8e28500-ac74-11ea-8cb2-9e804cdd3848.PNG)

