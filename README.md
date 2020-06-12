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
![ff](https://user-images.githubusercontent.com/66811679/84472764-ab4a2600-ac3c-11ea-9bcb-ff67b78a89b8.PNG)

2.Added The webhooks to the Jenkins Github API :=> ip/github-webhook/
![webhooks](https://user-images.githubusercontent.com/64473684/84473373-94d8b480-aca6-11ea-82bf-ecb0e698d38c.jpg)
## Screenshots :

## 1. Initially:

1. Production:

![n](https://user-images.githubusercontent.com/66811679/84483330-0f291a80-ac4e-11ea-9e8c-1237451ce415.PNG)

2. Testing :

![pm](https://user-images.githubusercontent.com/66811679/84483646-7fd03700-ac4e-11ea-9b34-408ef48c97a0.PNG)
## 2. Changed made but not approved:

1. Produnction:

![n](https://user-images.githubusercontent.com/66811679/84483330-0f291a80-ac4e-11ea-9e8c-1237451ce415.PNG)

2. Testing :

![pm](https://user-images.githubusercontent.com/66811679/84483646-7fd03700-ac4e-11ea-9b34-408ef48c97a0.PNG)


3. QA Rejected

## Changed and approved:

1. Testing :







