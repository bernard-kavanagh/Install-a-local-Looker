# Install-a-local-Looker

Contact

DCL Education




Intro
It's useful to have your own Looker instance that lives on your computer so that you can test stuff. It's also a good tool for learning about on-prem environments and the Looker directory structure.

Part 1: Submit a Helpdesk request for a new looker license. Indicate that this is a personal license to run on your laptop.
Part 2: Install Homebrew
to install homebrew on your mac, go to applications>utilities and open Terminal.
paste the following command:/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

Part 3: Install OpenJDK
( from Instructions - https://dzone.com/articles/install-openjdk-versions-on-the-mac)
enter the following commands in your terminal window:brew tap AdoptOpenJDK/openjdk 

brew search /adoptopenjdk/ 

brew cask install adoptopenjdk/openjdk/adoptopenjdk8

Part 4: Download Components
A Looker instance requires 3 things: two JAR files which have the code, a script file which runs the code, and a license key which gives you permission to run the code. We'll get all three of them here.

- Download both Looker jar files (main and dependencies) at this website. You'll need your license key from step 1. You can choose "Latest Version".

- Right click this link to the script: looker.txt. Choose "Save Link As", and save it as "looker.sh"

Next we want to get the jar and script into a new folder in your home directory. The next few steps will require some basic command line knowledge; follow the steps closely or take this short command line tutorial.

- Open up Terminal

- The cd command stands for "change directories" and is how you move around. Type cd now and hit Enter to go to your home folder.

- Create a new folder named "looker" by typing mkdir looker and hitting Enter.

- Move the jar files into this new folder. Assuming you saved it to Downloads, then you can do this with mv Downloads/looker-latest.jar looker/looker.jar. Do the same for the dependencies jar file (removing "-latest" from the name as you go).

- Move the script file into this new folder. Assuming you saved it to Downloads, then you can do this with mv Downloads/looker.sh looker/looker.sh.

- (For advanced users: note that the script makes assumptions about the exact path you are running the instance from. Comment or edit line 5 if needed.)

Part 5: Start Looker
To run Looker, simply start up the script, which will find the jar and serve Looker.

- Type cd looker and hit Enter to move into the folder you created in the last part.

- The script is read-only by default, but we can give it executable permissions. Type chmod +x looker.sh to add permissions to that script.

- Start Looker with the script by typing ./looker.sh start and hitting Enter.

- Wait until you see the message: Looker started successfully on port 9999.

Part 6: Use Looker
Now you can visit your local instance in the browser. Time to use that license key!

- Open a browser to https://localhost:9999 (NOTE the https).

- You'll probably see a privacy warning. This is because our instance is using a "self-signed" certificate that is not recognized by the web browser. You can still access it by clicking "Advanced" and then "Proceed".



- Now you should see a Looker page. Near the bottom there should be a link that says "Already have a license key?" Click that and enter your license key.

- You should now be inside Looker!

Part 7: Add a Database
- Go to https://localhost:9999/admin/connections/new

- Enter the following credentials:

    name: thelook
    dialect: MySQL
    host: db1.looker.com 
    port: 3306 
    database: demo_db
    username: looker_demo
    password: look_your_data

- If you want to enable PDTs, then check the "Persistent Derived Tables" checkbox and enter sandbox_scratch as the "Temp Database".

- If you want to use "User Specific Timezones" the Database Time Zone should be set to UTC.

- ENJOY!

Troubleshooting Notes:
- If you're getting permission errors, make sure you've granted permissions to the script with either chmod +x looker.sh or chmod 750 looker.sh. Then you can run ls -l to see the permissions of the files in the folder.

- You can only run one instance of Looker on your local machine at a time.

- To stop a running instance of Looker you can run the command ./looker.sh stop if you used the startup script, or java -jar looker.jar stop if you did not use the startup script.

- If you think that Looker is still running on your machine even after you told it to stop you can run ps -ef | grep java from the command line to find out for sure! This will tell you all the java processes running on your machine, which is probably only Looker, and you running your java query. If you do have more java processes running on your machine look for the one that contains something like "looker.jar".

- If you find that Looker is still running even after you have told it to stop, you will need to kill it! To do this run the command, kill [the_second_number_next_to_it].
