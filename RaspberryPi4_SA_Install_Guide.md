__Raspberry pi 4 (8gb) Superalgos setup + Bitcoin-Factory machine learning setup__
               



	Image flashed on micro SD card by Raspberry Pi Imager:      version: 1.7.2


    
Image used:
    		
             -   Raspberry Pi OS Lite (64 bit)
             -   release 2022-04-04
		




__1. Flash the above OS onto your micro SD using Raspberry Pi Imager__

        1.1 Select the correct Operating System.
        1.2 Select your micro SD card from the list of Storage devices.
        1.3 Click the settings button at the bottom right.
                Set Hostname, 
                enable SSH (if you want to use SSH), 
                Set username/Password, 
                Configure wireless LAN if needed.

        1.4 Write the image to the micro SD.



__2.  Time for first boot!__	 
- _This may take a few minutes... I used the pi connected to a monitor at this point._        

    - 2.1 After boot up finishes run following command to find local Ip address of your pi:
    	
          	ifconfig        
	

     - 2.2 Then SSH into the pi:
     	   
           	sudo apt-get update
		
	   ```
           	sudo apt-get upgrade

     - 2.3 Add user to sudo group: 
 
     	    
           	sudo su
		```
           	usermod -aG sudo yourPcUsername
		```
           	sudo reboot


       _(After each reboot SSH back into the Pi)_   
       

                                    

3.  Now lets install pre-requisites.

            (From where I land after SSH'ing into the pi again is where Superalgos is installed)


        3.1 First lets install git.
                                        sudo apt-get install git



        3.2 Now lets install node.js.
                                        curl -fsSL https://deb.nodesource.com/setup_17.x | sudo -E bash -

                                        sudo apt-get install -y nodejs



        3.3 Check your installs.
                                    node --version
                                    npm --version


                   ********************
				   * (Current Tested) *
				   *  node v17.9.0    *
				   *  npm 8.5.5       *  
				   *  Docker 20.10.15 *
                   ******************** 
        


    4. Now lets install docker.

                        (The following commands are taken from docs.docker.com)
            (URL = "https://docs.docker.com/engine/install/debian/#install-using-the-repository")


        4.1 Update:
                                    sudo apt-get update 

                                    sudo apt-get install \
                                        ca-certificates \
                                        curl \
                                        gnupg \
                                        lsb-release

        4.2 Add GPG key:
                                    curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg


        4.3 Set up the "stable" repo:       (copy paste all below text at once)

                                    echo \
                            "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/debian \
                            $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null


        
        4.4 Install Docker Engine:

                                    sudo apt-get update
                                    sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin


    4.5  Reboot again:
    				    sudo reboot
						    
						    


5.  Time to clone Superalgos and install it!

        5.1 Clone your fork as usual:
        
                                        git clone yourGitForkURL

        5.2 Now cd into Superalgos:
                                        cd Superalgos

        5.3 Run node setup:
                                        node setup    

        5.4 Run node setupPlugins:
        (I give repo, workflow,          node setupPlugins yourGitName yourRepoKey
            & write permissions
            to token)
    

        5.5 Change branches:
                                        git checkout develop

        5.6 Check everythings OK:
                                        git status



6. Set up docker image:

        6.1 Move to the ArmDockerBuild file:

                                        cd Bitcoin-Factory

                                        cd ArmDockerBuild


        6.2 Build the docker container:

                                        sudo docker build -t bitcoin-factory-machine-learning .


        6.3 Run the Docker Container:   (Copy paste all below at once)

                                        sudo docker run -d -it --rm --shm-size=4.37gb --name Bitcoin-Factory-ML -v ~/Superalgos/Bitcoin-Factory/Test-Client/notebooks:/tf/notebooks -p 8888:8888 bitcoin-factory-machine-learning		

                                        
						(the -d in the above command lets docker stay running when terminal is closed)



7. Run Superalgos. 
    (sudo is needed for Bitcoin Factory)
        
                                        sudo node platform minMemo noBrowser



5.  Any errors updating Superalgos?

                        Open Doc's tab and run "app.update"
                        Open Bitcoin-Factory workspace.
			
                        Error appears (Error1)
			
1. Picture from terminal of Error_1


	![Error1](https://raw.githubusercontent.com/theblockchainarborist/Superalgos-Guides/main/Error%20Pictures/Error_1.png)



				

  9. Also see following picture of error shown in contribute tab before fix.
    ![Error 2](![image](https://github.com/theblockchainarborist/Superalgos-Guides/blob/main/Error%20Pictures/Error_2.png?raw=true)

					
					

                        Open contributions tab- click "Reset"
                        Open Workspaces tab, then Bitcoin-Factory Workspace (again to test)
                                    Works!

                        


  *  ********************************************************************************************
  *  Everything all check out? Any problems the above does not solve when following this guide, *
  *  please let me know so I can add solutions in!                                              *
  *********************************************************************************************** 


      _________________________________________________________|_END SETUP_|________________________________________________________

