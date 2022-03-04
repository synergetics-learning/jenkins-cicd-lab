# Jenkins CI/CD Lab

Deploy Jenkins and Tomcat containers in group. Tomcat would be used as "deployment target" for the jenkins job.

> This lab environment can be deployed on `docker desktop` or `docker playground`.

## The Steps :

1.	Download [compose file](./docker-compose.yml) into an empty directory and then deploy 
	```
	# Validate the compose file
	docker compose config 
	# deploy all services
	docker compose up -d
	# List the services running
	docker compose ps
	```

	![Compose UP](./images/compose1.png)

1. Login into jenkins dashboard on port `10000` using following credentials
	
	```
	Username: mahendra
	Password: password@1234
	```

	![Jenkins Login](./images/jenkins-login.png)


1.	Login into tomcat manager on port `9000` with following credentials

	```
	Username: manager
	Password: manager
	```
	
	![Tomcat Manager Login](./images/tomcat-login.png)


1.	Now, in jenkins dashboard, create a new `Free style project` with Following details:

	```yml
	Project-Name: Job1
	Project-Type: Freestyle 
	Source-Control:
		git: https://github.com/mahendra-shinde/ci-servlet-demo
	Build-Triggers:
		Poll-SCM: 
			Schedule: " H/2 * * * * "
	Build:
		Invoke top-level Maven Target:
			Maven-Version: M3
			Goals: package
	```

1.	Click save button and then use `Build Now`

1.	Now, Open Jenkins Plugin Manager
	
	![Plugin Manager1](./images/jenkins-plugins1.png)

1.	Click on "Available" Tab and then search and install "deploy to container" plugin.

	![Plugin Intallation1](./images/plugins-deploy.png

1.	While installing the new plugin, make sure you `check` the option to `restart jenkins after plugin is installed`.

	![Plugin installation2](./images/plugins-deploy2.png)

1.	Go Back to `Job1` and click `Configure` button.

	![Job-configure](./images/job-configure.png)

1.	You need to add new `Post Build` action called `Deploy WAR/EAR to Container`

	![Deploy to Container](./images/deploy-step.png)

1.	Now, you need to use filename `target/*.war` an context path as `app1`, choose container `Tomcat 9`

	![Deploy-step2](images/deploy2.png)

1.	You need to add credentials for tomcat manager using the `Add` button and then `Jenkins`

	![Deploy-step3](images/deploy3.png)

1.	You need to add tomcat manager credentials

	```yaml
	Username: manager
	Password: manager
	ID: tom
	Description: Tomcat-Manager
	```

	![Deploy to step4](images/deploy4.png)

1.	Now, set the Tomcat Manager URL to `http://tomcat:8080/manager/text` and save the job.
	
	> The Tomcat Manager URL set to http://tomcat:8080/manager/text which is a private address only accessible to `jenkins` container as it is in same private network as `tomcat` container.
	W

	![Tomcat Manager](images/deploy5.png)


1.	Build the job manually and wait for success message.

1.	Verify The application deployment via Tomcat Manager `http://localhost:9000/manager`

	![Application Deployed 1](images/app-deploy.png)

1.	Click on the link `/app` to visit the website.

	![Application Deployed 2](images/app-deploy2.png)