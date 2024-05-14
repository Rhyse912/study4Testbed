# ASIST STUDY 4

  

ASIST Study 4 continues the legacy of Study 3 - leveraging Minecraft (1.11.2) as its Task Environment, but with some important differences. 

Rather than the evaluation of Socially Intelligent Agents, the objective of Study 4 is to design and build a generalizable, cloud-based testbed, suitable for collecting large quantities of experimental data in an automated fashion - with little to no administrative support. This data can then be used to train AI Models in the Human Machine Teaming domain.

To fulfill said objectives, the Study 4 testbed has been designed with study participants in mind, as opposed to administrators.
The burden of participant registration, login, and setup has been placed on the participants themselves. 

Much like a standard multiplayer online game, the testbed offers a Landing Page and Game Lobby - allowing players to asynchronously form teams and collect data on their own time.   

---

**Modes of Operation**

The ASIST Study 4 Testbed has two main running modes, **Single Instance** and **Scaling**.  

The **Single Instance** mode is a basic running mode that can be used to run 1 experimental trial at a time. This mode is useful for sequential evaluation and testing. The instructions below cover this mode of operation.
  

The **Scaling Mode** is significantly more complex, requiring integration with Cloud Hosting services that can dynamically spin up one Testbed Instance for each active team - enabling data collection for multiple teams in parallel. If you have questions about how to deploy such a system, contact the repo maintainer directly and they will do their best to respond and help you understand how the study was conducted.  

---

For information about Study 4 and its data ouputs, please visit https://artificialsocialintelligence.org

---  

### QUICK SETUP OF STUDY 4 ADMINLESS TESTBED 

Quick Setup - Linux/Windows

Requirements:

Docker Engine 20.0.0 +

Docker Compose v 2.0.0 +

Bash Shell

---  

**Important Note:**
  

Development has been done exclusively on Windows 10 and Alma Linux (release 8.8 Sapphire Caracal).
  

Linux-based Virtual Machines were hosted on the Google Cloud platform. This does not preclude you from using a different hosting service such as AWS or Microsoft Azure, though certain integrations will have to be adjusted to make those work. In summary, building and orchestration of the testbed may differ to some degree based on your deployment environment, but broadstrokes should be similar for all environments.  

Additionally, cloud hosting is not explicitly required to run the ASIST testbed. However, a powerful machine with at least 32CPU, 128 GB of RAM is recommended to run with a full suite of agents - as agents and individual components in this testbed are not optimized for performance. You may be able to run the bare-bones testbed instance with a 16CPU 64GB RAM system, but your mileage will vary depending on the amount of microservices you have concurrently active.

---


**Definitions:**  

**SSO Stack** - Single Sign On Stack - This is the docker stack represented by the Waiting Room, the Participant DB, Scaling Services, and the Single Point Sign On Service (Landing/Registration page) 

**AC** - Analytical Component - A pseudo "agent" whose purpose is to support an ASI by providing it certain metrics and measures that may help it to determine when to intervene, or signal a player  

**ASI** - Artificial Social Intelligence - The AI Agent that communicates to a player in the game world

**Experiment** - Experiments can be defined as any sessions in which an intact team stays together and plays the scenario at least once. An experiment has a number of attributes, defined by **MessageSpecs/Experiment**.  1 Experiment can have many Trials within it.

**Trial** - The individual play session of a team complete with metadata defined by **MessageSpecs/Trial**  

---

**Testbed Setup Steps:**

1. Pull the latest code from https://gitlab.com/artificialsocialintelligence/study4.git

2. Navigate to the Local directory, and open **./testbed_build_adminless.sh**. You can decide which containers you would like to build by either leaving, or commenting out the code-blocks that build individual services and agents. Depending on your needs, you may want to just bring up the testbed bare-bones, in which case all agent services (AC's and ASI's) may be commented out. This will significantly reduce build time. Depending on the number of containers being built, and the complexity of each container's build process, the full build may take anywhere from a few minutes to an hour. Once you are satisfied with the services you wish to be built in your testbed instance you can run `./testbed_build_adminless.sh`. This step may require a docker login if you are pulling from external repos to build AC or ASI components.  See here for instructions on using the docker login command --> [Docker Login Docs](https://docs.docker.com/engine/reference/commandline/login/).

The minimum necessary components required to have a functioning testbed are as follows:
	
	1. AsistControl - Should always be built, don't comment it out
	2. ClientMapSystem - Should always be built, don't comment it out
	3. WaitingRoom - Should always be built, don't comment it out
	4. SPSOService - Should always be built, don't comment it out
	5. logstash - Should always be built, don't comment it out
	6. metadata-docker - Should always be built, don't comment it out
	7. metadata-web - Should always be built, don't comment it out
	8. mqtt - Will be automatically started in Local/./testbed_up_adminless.sh
	9. ELK-Container - Will be automatically started in Local/./testbed_up_adminless.sh
	10. Minecraft - Will be automatically started in Local/./testbed_up_adminless.sh
  

3. Once you have successfully built all the docker images required for your needs, navigate to the **Local/WaitingRoom/WaitingRoomConfig.json** file, and set the number of players you would like to form a team. The field to edit is `team_assembly.players_per_team`. The current supported number of players per team is 1-3. Save your changes.

4. You should now set the passwords the system will require for operation. To do this, navigate to **export_env_vars_and_testbed_up.sh** - in the Local directory - and fill out the necessary passwords fields as shown below.
  

---

		export ADMIN_STACK_IP= the ip of the machine running the SSO Stack, in Single Instance mode this will be the ip address of the 1 machine hosting the testbed
		export ADMIN_STACK_PORT= the access port of the machine running the SSO Stack - defaults to 9000, but you can use 443 for standard https connections. This will require you to change the Local/NGINX/nginx.conf file  
		export DB_API_TOKEN= the access token used to contact SSO backend service and read, write, update, delete entries in the participant DB. You will need to create one of these after launching the testbed one time. The process for doing so will be explained after this set of instructions. On your first testbed launch this field can be left blank.
		export DB_API_ISSUER_KEY= a decent length key (we recommend at least 128 bit random key) to generate the above token
		export ADMIN_ID= The ADMIN User Id you would like to set for the Landing/Login Page
		export ADMIN_PWD= The password associated with the ADMIN User Id on the Landing/Login Page
		export PGADMIN_DEFAULT_EMAIL=The login username for PGADMIN
		export PGADMIN_DEFAULT_PASSWORD= The login password for PGADMIN
		export POSTGRES_PASSWORD= The POSTGRES Database password you wish to set
		export POSTGRES_USER= docker (leave this as is unless you know what you're doing)
		export POSTGRES_DB= userdb (leave this as is unless you know what you're doing)
		export DB_CONNECTION_PWD= The POSTGRES Database password you wish to set (should match POSTGRES_PASSWORD above - yes its redundant but that's how it is)
		export DB_CONNECTION_USER= docker (leave this as is unless you know what you're doing)
		export DB_CONNECTION_HOST=** userdb (leave this as is unless you know what you're doing)
		export DB_CONNECTION_PORT=** 5432 (leave this as is unless you know what you're doing)
		export DB_CONNECTION_DATABASE=** userdb (leave this as is unless you know what you're doing)
		export EMAIL_PWD=** If you have an app password from Google to enable automated emails from registration you may set it here
		export SSO_SECRET_KEY=** a decent length secret key to use in encryption/decryption of user account data
		export GCLOUD_KEYFILE=** If you have setup database backups, enter the provided KEY to access the Google Cloud bucket here
		export GCLOUD_PROJECT_ID=** Enter Google Cloud Project ID
		export GCS_BACKUP_BUCKET=** enter the address of the Google Cloud bucket( gs://db_backups.example.com)
---

---
**Example File**

        export ADMIN_STACK_IP=192.168.0.163
        export ADMIN_STACK_PORT=9000
        export DB_API_TOKEN=
        export DB_API_ISSUER_KEY=8oVyzyPabzvPc0eqK54DGxXC2taotA7k9L2ZBXuo9oZMkUXF5cLQ1X2VQfpdmrKT
        export ADMIN_ID=admin
        export ADMIN_PWD=HelloWorld2023
        export PGADMIN_DEFAULT_EMAIL=support@socialai.games
        export PGADMIN_DEFAULT_PASSWORD=HelloWorld2023
        export POSTGRES_PASSWORD=mB5aCU@Y5H!d
        export POSTGRES_USER=docker
        export POSTGRES_DB=userdb
        export DB_CONNECTION_PWD=mB5aCU@Y5H!d
        export DB_CONNECTION_USER=docker
        export DB_CONNECTION_HOST=postgres-db
        export DB_CONNECTION_PORT=5432
        export DB_CONNECTION_DATABASE=userdb
        exportEMAIL_PWD=lnvlpindupxgorsz
        export SSO_SECRET_KEY=KwUtT+GPLkz5p6GYKJqhq8Nl8qmrj4HDKsWeC6RyOZETKO19Y1RY5UpuRAmSJmZY
        export GCLOUD_KEYFILE=
        export GCLOUD_PROJECT_ID=
        export GCS_BACKUP_BUCKET=

        sleep 5
        sh ./testbed_up_adminless.sh -ar

---

5. You may now navigate to the **Local/testbed_up_adminless.sh script**. In this file, you may determine which agents and services are launched when you bring up the testbed ... much like **Local/testbed_build_adminless.sh**, you may either comment out code-blocks to skip services you don't wish to run, OR you may change the arguments provided to the **./testbed_up_adminless.sh** command at the bottom of the **Local/export_env_vars_and_testbed_up.sh** script file. The available arguments are explained at the top of the **./testbed_up_adminless.sh** script file.

![Testbed Up Args Example](docs/TutorialDocs/testbed_up_args.PNG  "Testbed Up Args Example")

6. Once you are satisfied that only the docker services you wish to run have been selected, go ahead and run `./export_env_vars_and_testbed_up.sh`. This will bring up the full testbed. This should generally not take more than a few minutes.
  

7. You may verify that all is working as you intended by visiting the Dozzle frontend @ https://your-deployment-address:9000/Logger/. If you've changed the Local/Nginx/nginx.conf file to route to 443, then this will be available at https://your-deployment-address/Logger/

8. You should a number of containers running as shown below:

![ASIST Dozzle Example](docs/TutorialDocs/Dozzle.PNG  "Dozzle Example")

9. Now it is time to generate the DB_API_TOKEN mentioned above. This can be done by sending an HTTPS post request to the SSO Service like so:

  
		ENDPOINT:   

		https://your-deployment-ip:9000/SSOService/user/authenticate	or https://your-deployment-ip/SSOService/user/authenticate` 

		BODY:  
		{  
		"UserId": "the admin user id you set above",  
		"Password": "the admin password you set above"  
		}  

		RETURNS:
		An object with a "token" key at the top level. This is your DB_API_TOKEN.

10. You can now bring down the testbed by running the `Local/testbed_down_adminless.sh`. Once the testbed is down, update the Local/export_env_vars_and_testbed_up.sh script with your newly obtained DB_API_TOKEN

11. Relaunch the testbed with the `Local/export_env_vars_and_testbed_up.sh` script

12. Follow the instructions for registering a player below.

---

**Registering a Player on the Testbed**

1. Navigate to the SPSO Registration page @ https://your-deployment-address:9000/SPSO/Registration and register a user.

2. Make sure to use an official Minecraft username exactly as it appears for the Minecraft avatar. When you log into the Minecraft Server you will be immediately disconnected if this does not match exactly.

![ASIST Register1 Example](docs/TutorialDocs/Register1.PNG  "Register1 Example")  

![ASIST Register2 Example](docs/TutorialDocs/Register2.PNG  "Register2 Example")  

3. On successful registration, you should see your username, participant id, and an automatically generated password presented to you in a popup. The automated email functionality is optional, so save the auto-generated password for your own records. You can always retrieve it again by logging in as an ADMIN later. Below is an example of the ADMIN view. By logging in as an ADMIN you can change any registrants email, password, or account name.

![ASIST ADMIN Example](docs/TutorialDocs/Admin1.PNG  "ADMIN Example")

4. These records are stored in the Participant DB ( a part of the SSO Stack ) as a docker volume, so removal of the docker volume will also remove all registered players. This particular docker volume is called **local_database_data**

## Playing the Game ##

1. Once you have successfully registered - navigate to the Login Page @ https://your-ip-address:9000/SPSO/Login. Go ahead and login.

2. Once you have acknowledged the consent form you will be given the option to download the ClientInstallationBundle, the Forge Mod, or instructions on how to download the system via CurseForge. This step only needs to be done once for each client installation, and is not necessary if your Minecraft Client is already setup with Forge and the Asist MOD.

![ASIST Consent Example](docs/TutorialDocs/ConsentForm.PNG  "Consent Example")

![ASIST Client Installation Example](docs/TutorialDocs/ClientInstallation.PNG  "Client Installation Example")  

3. At this time there are a few bugs in the ClientInstallationBundle system and we would NOT recommend using it. We do recommend either manual installation of Forge and the Mod, or installation of the Mod via CurseForge.
  

4. As of the writing of this document the mod version should be AsistMod 4.3.1. You can find the most recent mod @ Local/data/mods in this repo.
  

5. Once you have completed the Asist Mod installation process, you should find yourself in the Waiting Room.

![ASIST WaitingRoom1 Example](docs/TutorialDocs/WaitingRoom1.PNG  "WaitingRoom1 Example")

5. Follow the instructions presented in the dialog, and take any required surveys.

6. When enough players are in the Waiting Room to form a team (equal to the setting you entered in Local/WaitingRoom/WaitingRoomConfig.json), you will be sent a message indicating that you can "Proceed To The Experiment". Click "Proceed To Experiment" when you have been selected.

![ASIST Proceed Example](docs/TutorialDocs/Proceed.PNG  "Proceed Example")  

7. The player should now find themselves in the ClientMap. Once the server is loaded and all teammembers have also proceeded to the experiment, instructions will appear telling you how to continue with the game.

![ASIST ClientMap Example](docs/TutorialDocs/ClientMap.PNG  "ClientMap Example")   

8. The mission ends the players are given an exit survey.

9. Once the exit survey is complete, each player is asked whether they wish to **Continue With** or **Leave** the team. If each player choosed to continue, then the server will restart and a new Trial will commence. If even one player chooses to disband the team, then all players are sent back to the Waiting Room and the cycle repeats.

10. In Single Instance Mode, the testbed will bounce (turn them on and off) all running agents in between teams in order to create a clean session for them.

11. At the end of the Trial, all data is automatically exported, either to a folder local to the machine running the experiment, or to a predetermined location in the cloud (in our case a Google Bucket).  This functionality can be set in the **metadata/metadata-docker/matadata-app.env** file.

---

### Available Agents in this Repo

In the Agents folder, you will see a variety of subfolders that represent various AC or ASI microservice agents.

As far as having these agents available to you, agents fall into two categories:

1. Those that can be built from code directly
2. Those that must be pulled from external repositories

The following agents will be automatically built from code during the running of Local/testbed_build.sh:

1. AC_IHMC_TA2_Location-Monitor
2. AC_IHMC_TA2_Joint-Activity-Interdependence
3. AC_CMUFMS_TA2_Cognitive
4. AC_UCF_TA2_Flocking
5. AC_UAZ_TA1_DialogAgent
6. AC_CMU_TA1_PyGLFoVAgent
7. ASI_CMU_TA1_ATLAS
8. AC_Cornell_TA2_Cooperation
9. AC_Aptima_TA3_measures

The following agents require being pulled from an external repository before they can be instantiated in the testbed

1. AC_GALLUP_TA2_GEM - you must set the artifact location to pull this agent in **Agents/AC_GALLUP_TA2_GEM/settings.env** - field **DOCKER_IMAGE_NAME_LOWERCASE=**  .  If you cannot resolve the repo location of this agent, it is recommend you comment it out of the build script and the **Local/testbed_up_adminless.sh** script.  To get a copy of this image, check the **Agents/AC_GALLUP_TA2_GEM** folder for information about contacting Gallup.
2. ASI_DOLL_TA1_RITA - you must set the artifact location to pull this agent in **Agents/Rita_Agent/docker-compose.yml**. Notice the image definitions in each Rita service. You must set the docker image artifact location for the following services : **rabbitmq,clojure,tailer,mqt2rmq** . If you cannot resolve the repo location of this agent, it is recommend you comment it out of the build script and the **Local/testbed_up_adminless.sh** script. A copy of this agent's latest images is stored in the container registry  of this repository. For further information check the  **Agents/Rita_Agent** folder for information about contacting DOLL Labs.

---

### Creating News Updates Via PGAdmin

You may update the Landing page with news updates by logging into PGAdmin, connecting to the Participant DB, and creating a new entry in the News Table.

There are 3 columns in this table: NewsId (uuid), CreationDate (timestamp with timezone), and Content (text)

You may create a news update by generating a uuid, entering a timestamp with timezone, and text content in Markdown format as in the example below:

"fcd5c695-3c15-4f43-b96f-0c565bc62bfc"	"2023-10-26 00:00:00+00"	"## Required Mod Update 4.3.1 !"



### Installing the Minecraft client  

See `docs/ClientSetup.md` for instructions on how to set up the client.

---

### Developing Agents and Analytic Components

See [Agent Development Guide](Agents/agent_dev_guide.md)

---

### The ASIST SINGLE INSTANCE system diagram:

---

![ASIST system diagram](docs/asist_adminless_testbed_core_architecture.png  "ASIST Adminless Core Testbed Diagram")

---

### The ASIST SCALING system diagram:

![ASIST environment diagram](docs/asist_adminless_testbed_environment.png  "ASIST Adminless Testbed Environment Diagram")

---

### Additional Information

For more information about configuration of individual services in the testbed, navigate to that service's directory and look for a readme file. All services and scripts should have a somewhat detail explanation of their function, integration, and usage within their folders. If you have any questions, please feel free to reach out the the repository maintainer.
