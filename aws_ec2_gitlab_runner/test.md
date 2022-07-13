# Using EC2 as Gitlab Runner? But why?
### If you're familiar with that situation when...
- your pipeline didn't seem to want to start
- moved very slowly/need more power
- you consumed already the free tier of minutes...

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




<details><summary> 2) Install GITLAB runner  </summary>
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
 
 'sudo reboot'
  
 - right after reconnecting, check id docker is already running
 
 `systemctl status docker`
 
 - check if docker if working properly
 
 `docker run hello-world`
 
![Uploading image.png…]()

 
 
 
 
 
</details>



<details><summary> 3) Configure it to run with Gitlab  </summary>

</details>






