# __Raspberry pi 4 (8gb) Superalgos setup + Bitcoin-Factory ML setup__
	
Image flashed on micro SD card with:
	  ```
	  Raspberry Pi Imager:      version: 1.7.2
	  ```
	
	
Image used:
	   ```
	   Raspberry Pi OS Lite (64 bit)
	   release 2022-04-04
	   ```
	   
		




### __1. Flash the above OS onto your micro SD using Raspberry Pi Imager__
  - _(Just the regular raspberry pi Imager setup)_
    - 1.1 Select the correct Operating System.
    - 1.2 Select your micro SD card from the list of Storage devices.
    - 1.3 Click the settings button at the bottom right.
    	```
	  Set Hostname,
	  enable SSH (if you want to use SSH),
	  Set username/Password,
	  Configure wireless LAN if needed.
	  
     - 1.4 Write the image to the micro SD.



### __2.  Time for first boot!__	 
- _This may take a few minutes... I used the pi connected to a monitor at this point._        

    - 2.1 After boot up finishes run following command to find local Ip address of your pi:
    	  
          ifconfig
	  

    - 2.2 Then SSH into the pi:

	   ```     	   
	   sudo apt-get update
	   ```
	   ``` 
	   sudo apt-get upgrade
	   ```	   
		
     - 2.3 Add user to sudo group: 
 
     	    
           sudo su
	   ```
	   usermod -aG sudo yourPcUsername
	   ```
           sudo reboot


       _(After each reboot SSH back into the Pi)_   
       

                                    

### __3.  Now lets install pre-requisites__

  - _(From where I land after SSH'ing into the pi again is where Superalgos is installed)_

      - 3.1 First lets install git:
      
            sudo apt-get install git



       - 3.2 Now lets install node.js:
       	     
	     curl -fsSL https://deb.nodesource.com/setup_17.x | sudo -E bash -
	     ```
	     sudo apt-get install -y nodejs
	     ```


       - 3.3 Check your installs:
 	     ```
 	     node --version
	     ```
	     ```
	     npm --version
	     ```


                   ********************
				   * (Current Tested) *
				   *  node v17.9.0    *
				   *  npm 8.5.5       *  
				   *  Docker 20.10.15 *
                   ******************** 
        


### __4. Now lets install docker__
  - _(The following commands are taken from [docs.docker.com](https://docs.docker.com/engine/install/debian/#installation-methods))_

	- 4.1 Update:
 	     ```
	     sudo apt-get update
	     ```
	     ```
	     sudo apt-get install \
	     ca-certificates \
	     curl \
	     gnupg \
	     lsb-release
	     ```

	- 4.2 Add GPG key:
 	     ```
	     curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
	     ```


	 - 4.3 Set up the "stable" repo:
 	     ```
	     echo \
	     "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/debian \
	     $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
	     ```
	  
	 - 4.4 Install Docker Engine:
 	     ```
	     sudo apt-get update
	     ```
	     ```
	     sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
	     ```

	 - 4.5  Reboot again:
 	     ```
	     sudo reboot
	     ```


### __5.  Time to clone Superalgos and install it!__

  - _Just the usual setup steps!_
  
	- 5.1 Clone your fork as usual:
 	     ```
	     git clone yourGitForkURL
	     ```
	- 5.2 Now cd into Superalgos:
 	     ```
	     cd Superalgos
	     ```
	  
	- 5.3 Run node setup:
 	     ```
	     node setup
	     ```

	- 5.4 Run node setupPlugins:
	  - _(I give repo, workflow & write permission to token)_
	  <br>
	  
 	   ```
	   node setupPlugins yourGitName yourRepoKey
	   ```
	- 5.5 Change branches:
 	     ```
	     git checkout develop
	     ```
	- 5.6 Check everythings OK:
 	     ```
	     git status
	     ```
	- 5.7 Configure git:
		 ```
		 sudo git config --global user.name "yourGithubUserNameHere"
		 ```
		 ```
		 sudo git config --global user.email "yourGithubUserEmailHere"
	  
### __6. Set up docker image:__
  - _(The "-d" In the docker run command allows the container to run in the background and you to be able to reuse the terminal)_
  - _(For first setup / Troubleshooting it is recommended to omit the "-d" and use 2 consoles to run both Superalgos & Docker)_
  
    - 6.1 Move to the ArmDockerBuild file:
      ```
      cd Bitcoin-Factory
      ```
      ```
      cd ArmDockerBuild
      ```
      
    - 6.2 Build the docker container:
      ```
      sudo docker build -t bitcoin-factory-machine-learning .
      ```
  - _NOTE: If above build command fails try using the Convenience Script found [HERE](https://docs.docker.com/engine/install/debian/#install-using-the-convenience-script)_
  
    - 6.3 Run the Docker Container:
      ```
      sudo docker run -d -it --rm --shm-size=4.37gb --name Bitcoin-Factory-ML -v ~/Superalgos/Bitcoin-Factory/Test-Client/notebooks:/tf/notebooks -p 8888:8888 bitcoin-factory-machine-learning
      ```

### __7. Run Superalgos__ 
   - _(sudo is needed for Bitcoin Factory)_
   
      - Run node platform command:
        ```
        sudo node platform minMemo noBrowser
        ```

### __8.  Any errors updating Superalgos?__
  - _(Start with the first fix then try to find a picture that match's your error output at the console)_
    - 8.1 Open up the Doc's tab and update:
      ```
      app.update
      ```
     - 8.2 Open Bitcoin-Factory workspace:
       ```
       Error appears (See Error Picture 1 & 2)
       ```
			
- Error Picture 1 _(Console Output)_

  ![image](https://raw.githubusercontent.com/theblockchainarborist/Superalgos-Guides/main/Error%20Pictures/Error_1.png)

- Error Picture 2 _(Contribution tab view)_

  ![image](https://github.com/theblockchainarborist/Superalgos-Guides/blob/main/Error%20Pictures/Error_2.png?raw=true)
	

  - Open contributions tab- click "Reset"
  - Open Workspaces tab, then Bitcoin-Factory Workspace (again to test)
   - Fixed!         



  *  ********************************************************************************************
 - __Everything all check out? Any problems the above does not solve when following this guide?__
    
 - __Please let me know so I can add solutions in!__                                              
  *********************************************************************************************** 




      _________________________________________________________|__END SETUP__|________________________________________________________



