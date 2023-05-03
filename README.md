# Cloning the Repo
1. Clone this project in your local system.
2. Open the project in your favourite code editor.

# Setting up docker containers
3. Inside the editor open your integrated terminal and type ```docker ps ```. This should show you all the docker containers currently running.
4. If you already have some instances of grafana and influxdb running then close them with ```docker-compose stop```. If not then skip this step.
5. Now type ```docker-compose up -d ``` to run your compose file. ```-d``` is for detached mode so that you don't get continuous logs.


# Setting up influxdb
6. After starting up the containers, type ```docker ps``` in the same terminal window. Copy the CONTAINER ID of the influxdb container.
7. Type ```docker exec -it <CONTAINER-ID> bash```. Replace ```<CONTAINER-ID>``` with the ID of influxdb container which you copied in the previous step.
8. Now type ```influx``` in the terminal. Your influxdb is set up.
9. You can view your databases by typing ```show databases``` in the terminal.


# Creating a new database
10. Now we will create a fresh database for your project.
11. Type ```create <dbname>```, replace ```<dbname>``` with the name of your choice. 
12. Perform step 9 again. You should be able to see the database you just created in the output.
13. Type ```use <dbname>``` to enter into the database you just created.
14. Type ``` show series``` to see the contents of your database. It should display nothing since you just created it. To populate it we need to connect it to our k6 project and run the k6 scripts.


# Setting up Grafana Dashboards
1. Open ```http://localhost:3001```, replace ```3001``` with the port number you have set in ```ports``` filed under grafana in your docker-compose.yml file. If you didn't change anything just go with 3001.
2. Now go to Configuration (settings button) and click on Data Sources.
3. Click on 'Add data sources' and choose 'InfluxDB' option.
4. Here, give any name to your influxdb connection.
5. Under HTTP, provide the the url ```http://<systemip>:8087```, replace ```<systemip>``` with your system IP address. (See the last section on how to get system ip). If you have changed the port number(8087 in this case) then replace 8087 with the changed one else let it be.
6. Under InfluxDB Details, provide the name of the database you created above.
7. In User and Password field fill the same details as in your docker-compose.yml file under k6 > enviroment: INFLUXDB_DB_USER and INFLUXDB_DB_USER_PASSWORD. If it is empty then fill random details but make sure to fill the exact same details under 'InfluxDB Details' in grafana that you are currently setting up.
8. now click on 'Save & Test' in grafana.
9. You should get a message that 'Data source is working'.

# Creating a Dashboard
10. Now go to Create Dashboards option(Plus icon) in grafana and click on 'Add a new panel'
11. In Left side, there would be a Data Source option. Select the db that you created in your terinal.
12. Below that there would be a 'Select measurement' option. From there you can select any k6 metrics to plot in the graph. Currently it would be showing nothing because we ahven't run any k6 files yet.


# Running k6 Scripts
1. Open a new window in your editor's terminal.
2. Run a script file using this command ```kr run --out influxdb=http://localhost:8087/<dbname> <k6FileName>. Relplace <dbname> with the db you created inside influxdb and <k6FileName> with the path of the script file you want to run.
3. Now you would be able to see the k6 metrics in the Select measurement' option in grafana. Now you can plot any metrics of your choice.


# How to get up System ip
1. Open your terminal and type ```ifconfig```. In the bottom-most section there would be an ip address ahead of ```inet``` flag. That's your ip address.


# Errors

## InfluxDB Error: Bad Gateway
System Ip address is wrong. Get your ip address again using above mentioned steps and change it.    