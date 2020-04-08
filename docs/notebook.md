Date: 4/8/2020
ADD Iteration: 1
Specific Design Issue Addressed: Establish an Overall System Structure
Step 1: Establish Iteration Goal by Selecting Drivers
For the first step of ADD step 1, we need to consider the drivers for our system.
To identify our drivers, we need to analyze our quality attribute scenarios, conerns, roles, legal issues and constraints. Doing this will help us understand
the drivers of our system.
By referring to the M1 Initial Drivers Document, we can identify the following drivers:
Performance
	QAS1: When a supplier upgrades a single-capsule coffee machine using the Simple Coffee Controller, the upgrade should take no more than 10 minutes.
	QAS2: When a supplier upgrades a commercial coffee machine using the Advanced Coffee Controller, the upgrade should take no more than two hours. 
	QAS3: When a new device comes online, CM2W will automatically map it to the client’s account within two minutes. 
	QAS5: When a user dispenses coffee from a coffee machine during the morning rush, the coffee machine should push updates without any information loss and without interrupting the coffee machine itself.
Availability
	QAS4: If cell service goes down, then the controller software should log uses offline until cell services comes back up, then sync the updates all at once without information loss and without interrupting the coffee machine.
	