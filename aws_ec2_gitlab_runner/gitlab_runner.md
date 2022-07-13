# Using EC2 as Gitlab Runner? But why?
### If you're familiar with that situation when...
- your pipeline didn't seem to want to start
- moved very slowly/need more power
- you consumed already the free tier minutes allocation...

### Then you might want to try using AWS resources for your Gitlab jobs.

# Steps: 

<details><summary> 1) Create an EC2  </summary>

 ### Prerequisites:
 - active AWS account preferably in the free tier period, as you have free access to EC2 t2 micro instance type
 
 ### Actions:
 - From the EC2 page, click on 'Launch instance' (region is not important)
 ![image](https://user-images.githubusercontent.com/86648102/178793158-06589ec0-906b-4e95-969d-e054b8af3347.png)
 - name the instance
 - choose Ubuntu
![image](https://user-images.githubusercontent.com/86648102/178793705-845ccf75-b4f7-4790-b295-8c7f3acd73f6.png)
 - let instance type as 't2 micro'
 - click on 'Create new key pair'
 ![image](https://user-images.githubusercontent.com/86648102/178794155-f2e0144c-e475-4066-819a-8260a8760649.png)
 - give it a name
 - let the other settings as default and click on 'Create key pair'
 ![image](https://user-images.githubusercontent.com/86648102/178794507-299359eb-ae89-4d94-a5f9-7258d991a7cd.png)
 - a pop up window will appear:
 - select 'Save File' and 'Ok'
 ![image](https://user-images.githubusercontent.com/86648102/178794704-589700ae-6c6f-4de7-bf3d-e75b32724fb5.png)
 - you can now click on 'Launch Instance' ; let the other settings as default for now
 ![image](https://user-images.githubusercontent.com/86648102/178795039-936f97db-424b-4029-a6e2-354c8a013c6e.png)
 - very shortly, in your EC2 main page, you will see your instance in 'Running state'; select it and press on 'Connect'
 ![image](https://user-images.githubusercontent.com/86648102/178795549-4e926bbf-8c3a-4f8c-816f-6ed6cea78185.png)
 
 ![image](https://user-images.githubusercontent.com/86648102/178799962-6a529786-967a-4dbf-9471-c9e00461dffd.png)
 
</details>




<details><summary> 2) Install dependencies </summary>
Now that we have access to the instance(terminal), it's time to configure it.
 
### Actions:
 
 - update the machine
 
`sudo apt update -y && sudo apt upgrade -y`
 
 - check if git is available:
 
 `git --version`  
 
 - if not available use the following command to install it:
 
 `sudo apt install git`
 
 - install docker
 
 `sudo apt install docker.io`
 
 - add our user to the docker group
 
 `sudo usermod -aG docker $USER`
 
 - make sure docker will start automatically with the instance
 
 'sudo systemctl enable docker'
 
 - reboot our instance (wait 2 3 minutes, then reconnect from EC2 window)
 
 `sudo reboot`
  
 - right after reconnecting, check id docker is already running
 
 `systemctl status docker`
 
 - check if docker if working properly
 
 `docker run hello-world`
 
![image](https://user-images.githubusercontent.com/86648102/178800278-a0c29642-17c9-4e74-b1f1-f72ad87151a3.png)

</details>



<details><summary> 3) Install GITLAB runner </summary>
Our machine has now the base software needed. It's time to continue.
 
 ### Actions:
 - download the gitlab-runner
 
 `sudo curl -L --output /usr/local/bin/gitlab-runner "https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-linux-amd64"`
 
 - give it permission to execute
 
 `sudo chmod +x /usr/local/bin/gitlab-runner`
 
 - create a Gitlab CI user:
 
 `sudo useradd --comment 'GitLab Runner' --create-home gitlab-runner --shell /bin/bash`
 
 - install and run as service:
 
 `sudo gitlab-runner install --user=gitlab-runner --working-directory=/home/gitlab-runner`
 
 - check if working and enable it so it starts automatically with the machine (same we did with docker)
 
 `systemctl status gitlab-runner.service`
 `sudo systemctl enable gitlab-runner.service`
 
 </details>


<details><summary> 4) Configure it to run with Gitlab  </summary>
 Time for some settings in your Gitlab account
 
 ### Actions:
 
 - create Gitlab group (so the runner will be available for the whole group, not just for a user)
 ![image](https://user-images.githubusercontent.com/86648102/178804107-508d829b-dc4d-4ffc-9e04-9c91534bf4e3.png)

 - name the group, set it as Private and scroll down and click on 'Create group'
 
 ![image](https://user-images.githubusercontent.com/86648102/178804364-4ac6db8b-509f-4f5a-befc-4310b16d757a.png)

 - click Settings >> CI/CD
 
 ![image](https://user-images.githubusercontent.com/86648102/178804646-0e826a2c-cc70-42c0-be10-8a77e01db343.png)

 - expand the 'Runners' section, disable 'shared runners for this group' and click on 'Take me there' to the new group runners view
 
 ![image](https://user-images.githubusercontent.com/86648102/178804968-243faa58-7be2-4a07-9bda-6d396af31ab8.png)
 ![image](https://user-images.githubusercontent.com/86648102/178805121-d33be8f5-1137-4633-b5b1-f43fd5f35be7.png)

 - now back on the EC2 terminal, run the following:
 
 `sudo gitlab-runner register`
 
 - and enter `https://gitlab.com/` as GitLab instance URL 
 - and the token from the website as registration token
 
![image](https://user-images.githubusercontent.com/86648102/178806007-e93862e7-a1f9-4cd5-9988-edb80bcad7af.png)

 - an optional description : `testing_aws_ec2_runner`
 - tags : `testing_aws_ec2_runner`
 - optional maintenance note : `testing_aws_ec2_runner`
 - enter an executor: `docker`
 - default docker image : `alpine`
 
 Congrats! By now your runner should be registered successfully.
 
 ![image](https://user-images.githubusercontent.com/86648102/178806754-459597f0-4c33-4fee-ba1b-6cfaa0140b36.png)

 - one more step here...check the runner status
 
 `sudo gitlab-runner status`
 
 - let's get back to Gitlab page and on our runner, click 'Edit' 
 
![image](https://user-images.githubusercontent.com/86648102/178807302-62c9b9f4-0144-4f8e-9a6b-fcbc129a34b3.png)

 - tick the 'run untagged jobs' and 'Save'
 
 ![image](https://user-images.githubusercontent.com/86648102/178807458-30eafeed-1e9e-409c-9ec4-1e5990405e3d.png)

</details>

<details><summary> 5) Test our runner  </summary>
 Ok, we have it set...let's put it to work.
 
 ### Actions:
 
 - from our group, create on 'New project'
 
 ![image](https://user-images.githubusercontent.com/86648102/178808007-d9ed3c22-43d7-40ed-8cae-3f600caf733f.png)

 - select 'Create blank project'
 
 ![image](https://user-images.githubusercontent.com/86648102/178808080-44d0a726-4045-4961-89a5-b231e53f68d8.png)

 - name the project, add a description(optional) and keep it as Private
 
 ![image](https://user-images.githubusercontent.com/86648102/178808341-8c11b69c-0c65-4ccb-8bb9-6687ab098025.png)

 - on the next window, create New File  
 
 ![image](https://user-images.githubusercontent.com/86648102/178808455-da3c0944-5b53-4669-8404-f188c7cd0fe8.png)

 - name the file, select 'gitlab-ci.yml' ,add the following job snippet and commit:
 
 ```
 build:
    image: alpine
    script:
        - echo "Hello" > index.html
    artifacts:
        paths:
            - index.html
 ```
 
 - hover over 'CI/CD' > Pipelines
 
 ![image](https://user-images.githubusercontent.com/86648102/178809190-6d6e2a55-43ee-4a15-a5e3-fe7c9e5843e3.png)

 - our build finished successfully
 
 ![image](https://user-images.githubusercontent.com/86648102/178809411-b38e1ca4-bf61-49a8-82a3-4255588f3b35.png)

 - let's see some logs; all has been done just as we set it
 
 ![image](https://user-images.githubusercontent.com/86648102/178809938-59bc6ea8-6dd2-4f80-abed-d321a41739ff.png)

 - artifacts are available for downloading:
 
 ![image](https://user-images.githubusercontent.com/86648102/178810181-32e60bab-2d5d-40a2-8098-c3835121633e.png)
 
</details>
 
<details><summary> 6) Congratulations  </summary>
 
 ### Thank you for getting to this point; now you have a Gitlab runner configured on an EC2 instance! Let me know if you got issues while doing any of the steps!
 
</details>
