Docker Oracle Database Image with Encapsulated Data
===============
Allows a docker oracle database image to be created from maven, with data encapsulated specifically to the container.  Allows the 
image to also be run from project through a maven command

## QuickStart: How to build and run

### Building Oracle Database Docker Install Images
**IMPORTANT:** You will have to provide the installation binaries of Oracle Database and put them into the `OracleDatabase/dockerfiles/<version>/installer` folder. You only need to provide the binaries for the edition you are going to install. You also have to make sure to have internet connectivity for yum. Note that you must not uncompress the binaries. The script will handle that for you and fail if you uncompress them manually!

Before you build the image make sure that you have provided the installation binaries and put them into the right folder.

Build the image:

	[oracle@localhost OracleDatabase]$ mvn exec:exec
	
**IMPORTANT:** The resulting images will be an image with the Oracle binaries installed. On first startup of the container a new database will be created, the following lines highlight when the database is ready to be used:

	#########################
	DATABASE IS READY TO USE!
	#########################
	
### Running Oracle Database in a Docker container

#### Running Oracle Database Enterprise and Standard Edition in a Docker container
To run your Oracle Database Docker image use the **docker run** command as follows:

	docker run --name <container name> \
	-p <host port>:1521 -p <host port>:5500 \
	-e ORACLE_SID=<your SID> \
	-e ORACLE_PDB=<your PDB name> \
	-v [<host mount point>:]/opt/oracle/oradata \
	oracle/database:12.1.0.2-ee
	
	Parameters:
	   --name:        The name of the container (default: auto generated)
	   -p:            The port mapping of the host port to the container port. 
	                  Two ports are exposed: 1521 (Oracle Listener), 5500 (OEM Express)
	   -e ORACLE_SID: The Oracle Database SID that should be used (default: ORCLCDB)
	   -e ORACLE_PDB: The Oracle Database PDB name that should be used (default: ORCLPDB1)
	   -v             The data volume to use for the database.
	                  Has to be owned by the Unix user "oracle" or set appropriately.
	                  If omitted the database will not be persisted over container recreation.

Once the container has been started and the database created you can connect to it just like to any other database.

The Oracle Database inside the container also has Oracle Enterprise Manager Express configured. To access OEM Express, start your browser and follow the URL:
	https://localhost:5500/em/

#### Changing the admin accounts passwords
The password for accounts can be changed via the **docker exec** command. **Note**, the container has to be running:

	docker exec <container name> ./setPassword.sh <your password>

#### Running Oracle Database Express Edition in a Docker container
To run your Oracle Database Express Edition Docker image use the **docker run** command as follows:

	docker run --name <container name> \
	--shm-size=1g \
	-p 1521:1521 -p 8080:8080 \
	-v [<host mount point>:]/u01/app/oracle/oradata \
	oracle/database:11.2.0.2-xe
	
	Parameters:
	   --name:     The name of the container (default: auto generated)
	   --shm-size: Amount of Linux shared memory
	   -p:         The port mapping of the host port to the container port.
	               Two ports are exposed: 1521 (Oracle Listener), 5500 (OEM Express)
	   -v          The data volume to use for the database.
	               Has to be owned by the Unix user "oracle" or set appropriately.
	               If omitted the database will not be persisted over container recreation.


## License
To download and run Oracle Database, regardless whether inside or outside a Docker container, you must download the binaries from the Oracle website and accept the license indicated at that page.

