# ADD Iteration: 1

Date: 4/8/2020 - 4/11/2020

## Step 1: Review Inputs

Lets begin by reviewing the inputs of our system and defining which requirements we will consider as drivers.

Design Purpose- The purpose of this design is to explore different prototypes to address CM2W's desire to spread their IoT capabilities into the coffee industry.

#### Side Note:

For ADD step 1, we can pretty much get all of our information straight from the M1 Initial Drivers Document

### Primary Functional Requirements
	UC1: User makes coffee on a small/medium/large machine
	UC2: Client checks how much coffee is left remotely
	UC3: Client orders more coffee supplies for multiple machines
	UC4: Supplier predicts when next coffee order will be needed
	UC5: Client describes business rules locking/unlocking users’ access to coffee machine
### Quality Attribute Scenarios
	QAS1: When a supplier upgrades a single-capsule coffee machine using the Simple Coffee Controller, the upgrade should take no more than 10 minutes.
	QAS2: When a supplier upgrades a commercial coffee machine using the Advanced Coffee Controller, the upgrade should take no more than two hours.
	QAS3: When a new device comes online, CM2W will automatically map it to the client’s account within two minutes.
	QAS4: If cell service goes down, then the controller software should log uses offline until cell services comes back up, then sync the updates all at once without information loss and without interrupting the coffee machine.
	QAS5: When a user dispenses coffee from a coffee machine during the morning rush, the coffee machine should push updates without any information loss and without interrupting the coffee machine itself.
### Constraints
	1. Controller software must be written in C.
	2. Simple Coffee Controller hardware cannot dispense coffee, but can do everything else the Advanced Coffee Controller can.
	3. Can't poll coffee machines for status. Need to wait for them to push status.
	4. All APIs must have no application state.
### Concerns
	Technology Concerns
		1. Two Point of Sale controllers being worked on: Simple Coffee Controller and Advanced Coffee Controller. Software needs to support both.
		2. Controllers connect to Internet over HSPDA. 
		3. Prefer to use a REST API over HTTPS. 
		4. Combine SQL and NoSQL databases in order to make our platform highly scalable. 
		5. Need on-premise Android 3.0 apps to automatically dispense coffee, plus iOS 7 + Android 3.0 app for checking stocks of all of client's machines. 
	Roles
		1. Suppliers, Clients, Users, and CM2W Admin 
		2. Only Clients can set business rules for controlling access and automatic re-ordering 
		3. Only Clients can manually request more inventory 
		4. Suppliers can only see stocks for machines that the Client specifies 
		5. Users only have permission to brew coffee at the machine by default, but can be given ability to see stocks for individual machines and ask a machine to make a coffee using an on-premise tablet app
		6. Only Clients can authorize CM2W Admins to access their data, either through the app or over the phone 
	Legal/data privacy issues
		1. Orders for more supplies need to be kept private so people don't raid the office supply when a new order comes in.
		2. Shouldn't be able to predict how much coffee is used just by looking for network traffic from the coffee machine. 
		
## Step 2: Establish Iteration Goal by Selecting Drivers

For the first step of ADD step 2, we need to consider the drivers of our system.
To identify our drivers, we need to analyze our quality attribute scenarios, conerns, and constraints. Doing this will help us understand
the drivers of our system and what approaches can initially be taken to solve them.
By referring to the M1 Initial Drivers Document, we can identify the following drivers for our first iteration:

### Performance
	1. QAS1
	2. QAS2
	3. QAS3
	4. Concerns - Technology Conern 3
### Availability
	1. QAS4
### Security
	1. QAS5
	2. Concerns - Roles 6
	3. Concerns - Roles 5
	4. Concerns - Legal/data privacy issues 1
	5. Concerns - Legal/data privacy issues 2
### Modifiability
	5. Concerns - Technology Concerns 4
	
## Step 3: Choose One or More Elements of the System to refine

Since this is greenfield development, we will be refining the entire CM2W coffee system

## Step 4: Choose One or More Design Concepts That Satisfy the Selected Drivers

Because this is our first iteration of the ADD process, our goal will be to establish an initial overall system structure. To do so, we will be considering
our first reference architectures and a module view to satisfy our drivers.

### Design Decision: Logically structure our user based system using the Mobile Application Architecture

Rationale: The 5th technological concern mentions that our system "Needs on-premise Android 3.0 apps...., plus iOS 7 + Android 3.0 app for checking stocks
of all client's machines." That reason alone should be convincing enough for us to strongly consider the mobile application architecture, but there are
other drivers that we should consider as well. For instance, if the user wants to be able to check how much coffee they have left remotely, then we don't want
them to have to carry around their laptop all day with them. What if they work at a job where you aren't close to your laptop? Therefore, a mobile application
would be helpful in the sense that it will allow clients to remotely make coffee or check how much coffee is left (QAS1 + QAS2).
Additionally, according to external research, mobile applications make it very easy to use REST APIs rather than HTTPS (Technical concerns - 3). Finally,
using a mobile application will help addresses the ability for the client to authorize CM2W Admins to access their data over an app (Concerns - Roles 5).

External Research: https://savvyapps.com/blog/how-to-build-restful-api-mobile-app


## Step 5: Instantiate Architectural Elements, Allocate Responsibility, and Define Interfaces

### Design Decision and Location: Add SQL and noSQL databases for the Mobile Application Architecture

Rationale: In order for our mobile application to be highly scalable and address our Modifiability driver for this iteration, we need to use SQL and noSQL
database platforms for our data source in the Mobile Application Reference Architecture.

External Research: N/A

### Design Decision and Location: Add security layers to the Mobile Application Architecture's that hide network traffic and office supplies

Rationale: This decision comes from our two concerns regarding privacy and legal concerns (Concerns - Legal/data privacy issues 1 and Concerns - Legal/data privacy issues 2)
Adding these security layers to our architecture will allow us to address these concerns; and now clients wont have the ability to immediately buy up all the
coffee ingrediants once they are shipped in. Additionally, users have their privacy ensured as their network traffic will not be monitored from a specific
coffee machine.

External Research: N/A

## Step 6: Sketch Views and Record Design Decisions

Initial Module view of the system (Module View):

![Mod View](images/InitialDomainModel.png)

Going off of this, we will define our first reference architecture for this system.

### Mobile Application Architecutre
![Mod View](images/AppArchitecture.png)

### Tactics Considered

#### Performance
	1. Control resource demand: This was largely addressed with my system by quickly accounting for two databases. Doing this allows us to properly manage
	where the data of the system is going. This will help reduce the traffic for database access.

#### Security
	1. Resisting attacks: So this was a little hard to incorporate with the initial module view, but I ended up putting a security layer in the system in hopes
	that I would elaborate on it in future iterations to provide a layer against network attacks. Also, something internal, having a permissions layer is good
	general security that will help ensure the users of the system are doing only what they should be doing.
	
## Step 7: Perform Analysis of Current Design and Review Iteration Goal and Achievement of Design Purpose

| Addressed| Partially Addressed | Not Addressed  | Decisions Made during Iteration |
| :---     | :---                |     :---:      |          ---: |
|  UC1     |                     |                |  Using the CM2W Management platform allows users to order their different sizes of coffee from different machines   |
|  UC2     |                     |                |   The mobile application reference architecture allow the client to remotely check how much coffee is left    |
|  UC3     |                     |                |   The CM2W Management platform will allows clients to make orders for their different machines    |
|  UC4     |                     |                |  I already put a security layer in my module view to prevent supplies from being raided as soon as they arrive    |
|          |                     |         UC5    |   This will need to be incorporated in the business logic of the reference architecture I have provided, but it hasn't been explicitly provided yet    |
|  QAS1    |                     |                |   Added a timer into the module view of our system; this will make sure that upgrades using the Simple Coffee Controller are done within the 10 minute timeframe    |
|  QAS2    |                     |                |   Again, will be using the timer mentioned for QAS2 to make sure that the upgrade, this time using the Advanced Coffee Controller, will take less than 2 hours    |
|  QAS3    |                     |                |   We have the CM2WNewDeviceIntegration class in our system so that new device logic is handled properly. We will use the timer again to ensure we meet our time constraint.    |
|  Tech-Concern 1    |                     |                |   Both SimpleCoffeeController and AdvancedCoffeeController classes have been added to our system in order to support both systems    |
|      |                     |         Tech-Concern 2       |   Not yet addressed    |
|      |                     |         Tech-Concern 3       |   Not yet addressed    |
|  Tech-Concern 4    |                     |                |   Added SQL and NoSQL database access to the data source of the module view    |
|      |         Tech-Concern 5            |                |   PointOfSalesMobileApp is not specified by Android or iOS, but at least gives us a starting point to implement those specific versions    |
|  Roles-Concern 1    |                     |                |   PermissionsLayer will control different roles of the system and what user will have access to what features of the CM2WManagementPlatform   |
|      |                     |       Roles-Concern 2          |  Not yet addressed   |
|      |                     |       Roles-Concern 3          |  Not yet addressed   |
|      |                     |       Roles-Concern 4          |  Not yet addressed   |
|      |                     |       Roles-Concern 5          |  Not yet addressed   |
|      |                     |       Roles-Concern 6          |  Not yet addressed   |
|   Legal-Concern 1   |                     |                |   CM2WSecurityLayer will ensure that our system's order are hidden so that clients don't hoard orders up front    |
|   Legal-Concern 2   |                     |                |   Our system has both the SimpleCoffeeController and AdvancedCoffeeController tied to our security layer, which will prevent network traffic data from being broadcast or used to predict which station is most popular    |
|      |                     |         Constraint 1       |   Not yet addressed    |
|      |                     |         Constraint 2       |   Not yet addressed    |
|      |                     |         Constraint 3       |   Not yet addressed    |
|      |                     |         Constraint 4       |   Not yet addressed    |

# ADD Iteration: 2

Date: 4/11/2020 - 4/12/2020

## Step 1: Identifying Structures to Support Primary Functionality

I think it is important here to realize that I left a lot of the design  open to interpretation from ADD Iteration 1. In this iteration, I will be
narrowing down on structures that I know will specifically address the concerns, constraints, and use cases that were not satisfied in my previous iteration.
Our goal for this iteration is to begin thinking about what specific units we will use to implement our system. This will allow us to further divide the work
up into teams so that we have people working on specific areas of the system that they are trained for when development begins. I will additionally do another reference architecture
for consideration in this milestone. Finally, I should note that a lot of addressed drivers in the previous milestone need methods to specifically address the
driver. Therefore, those methods will be added in this iteration.

## Step 2: Establish Iteration Goal by Selecting Drivers

Again, we need to be looking at what needs to be done in order to support the primary functionality of our system. I think that it makes sense to address the
concerns of our system that were not satisfied in ADD iteration 1. The reason for this being they are critically important to the primary functionality of
the system. Therefore, in this iteration we will be looking to address the following drivers:

	
### Usability
	1. Technicial Concern 5
	2. Roles Concern 2
	3. Roles Concern 3
	4. Roles Concern 4
	5. Roles Concern 5
	6. Roles Concern 6
	
## Step 3: Choose One or More Elements of the System to Refine

In this iteration, we will be considering another reference architecture . I will go into detail on that in the next step, but I also want to add methods to
our domain model so it looks more like a Class Diagram. This will give us a better understand of the functionality of each part in our system. I will first consider
another architecture, and then move on to creating a Class Diagram from our Domain Model.


## Step 4: Choose one or More Design Concepts That Satisfy the Selected Drivers

### Design Decision: Logically structure our user based system using the Rich Internet Application Architecture

Rationale: It should be pretty clear that our system will heavily rely on the internet for several operations like giving the client the ability to check
how much coffee is left remotely (UC2), allowing the client to order more coffee supplies (UC3), and automatically mapping new devices to the client's account
when it comes online within two minutes (QAS3). Also after doing some research and going further into the project, I realized that rich internet applications are especially helpful for mobile access,
integration among many systems, and effective data visualition. These are absolutely critical for the CM2W coffee system, and which is why I am also considering
it as another reference architecture.

External Research: https://www.manufacturing.net/home/article/13055754/5-benefits-of-rich-internet-applications-for-manufacturing-roi

### Design Decision: Convert the module view of our system into more of a domain model so that user objects and methods can be  defined


By adding specified user objects and methods to our system, it will make our system much easier to handle different permissions and allow users to have functionality
that is different based on what the user is actually supposed to do. One benefit to this approach is that we already have our user PermissionsLayer specified,
now all we need to do is add our users. Additionally, since we are converting into a class diagram, this will allow us to add some methods that are critical
for our system to function as needed. This will include some of the functions that we mentioned we needed to add in our first ADD iteration.

## Step 5: Instantitate Architetural Elements, Allocate Responsibilities, and Define Interfaces

| Design Decision & Location| Rationale |
| :---:                     |          :---: |
| Improve initial domain model with user objects | This will allow us to specifictly identify and address the issues presented to us, with regards to Role Concerns, in our drivers for this iteration |
| Introduce two new objects in the presentation layer - one for Android and iOS | Having two models that represent Android and iOS platforms allows us to also finish addressing Technical Concern 5, which was only partially addressed last iteration |
| Add methods to from ADD iteration 1 to domain Model | Adding methods allows us to more clearly define what the main business logic of our system will be doing |

## Step 6: Sketch Views and Record Design Decisions

### Rich Internet Application Architecture
![Mod View](images/RIArchitecture.png)

After developing the architecture for the rich internet application, I have decided more of the system should be designed for a mobile application, and so from
here on our, I will base my architectures on that. I am simply doing this because there is not a lot of fundamental architectural different between the two,
and mobile applications also seem to satisfy more drivers that rich internet applications. Additionally, I just believe it makes overall sense to use a mobile
app rather than a Rich Internet Application. Ordering coffee is something that companies like Starbucks use mobiles apps for, and was the best selling Apple
app until recently. More people are using apps to order coffee that their computers or phone browsers, so we're gonna stick with that to have a larger
customer base and address more drivers.

External Research: https://www.emarketer.com/content/apple-pay-overtakes-starbucks-as-top-mobile-payment-app-in-the-us

Class Diagram: 

![Mod View](images/DomainModelMethods.png)

#### Important Side Note:

These methods are open for change. I am really just trying to get a better idea for when I go to create finalize the Class Diagram. These methods
are simply here to get a general idea of where the functionality of the system will be distributed
and what classes will be responsible for what actions.

| Element | Responsibility |
| :---:  |          :---: |
|     DefinedUser    |        I made this an enum to signify that it was unchanging and that the only users of the system would be the User, the client and the supplier         |
|  Android/iOS Controller Support |     I was less sure on how to distinguish between the controllers for Android and iOS devices, so I just added that note. When the dev team goes to make the apps, they will know that both controllers will have to be different to adjust to the CM2WManagementPlatform      |
| Android/iOS GUI Support  |   Similarly, the GUI implementations are different for Android and iOS devices, so I put annotations in so the developers of the system know that        |
| OperationalManagement  |   This is here if CM2W every wants to change user permissions or define what users are able to do; this isn't spelled out by any driver but I thought it was good to include  |


## Tactics Considered

#### Usability
	1. Supporting system initiative. Have both Android and iOS GUI & controller support means that users can access the CM2WManagementPlatform on both Android devices and
	Apple devices
	

## Step 7: Perform Analysis of Current Design and Review Iteration Goal and Achievement of Design Purpose

#### Important Side Note: 

I defined all of the user classes (USER, CLIENT, SUPPLIER) in the actual class diagram. So when it comes to having users defined, that is what I did. That way I would not have
to list it in literally every table entry below. This means that users can access user-related functions through the PermissionsLayer's setDefinedUser() function, clients can access client-related functions
through the PermissionsLayer's setDefinedUser() function, etc. SetDefinedUser() will set the user and grant them functions mentioned in CM2WManagementPlatform.

| Addressed| Partially Addressed | Not Addressed  | Decisions Made during Iteration |
| :---     | :---                |     :---:      |          ---: |
|  Roles Concern 2 & UC5        |                     |                |    In order for the clients to be able to set the business rules for controlling access, I've put in a function in the PermissionsLayer called setBusinessRules()                             |
|  Roles Concern 3        |                     |                |    Similarly, I put a function in the CM2WManagementPlatform called requestMoveInventory()                              |
|  Roles Concern 4        |                     |                |    There is now seeStocks() in the CM2WManagementPlatform                             |
|  Roles Concern 5        |                     |                |    I added a function called askToMakeCoffee() in the CM2WManagementPlatform, which applies to users                  |
|  Roles Concern 6        |                     |                |    There is another function called authorizeAdminsAccess(), but I put this one in PermissionsAccess instead of CM2WManagementPlatform because it pertains to granting permissions to users                              |
|  Technical Concern 5        |                     |                |    Added Android and iOS annotations for both the domain layer and the presentation layer to handle both the Model and the Controller differences between Android and iOS apps                              |
|          |                     |        Constraint 1        |           Not addressed                       |
|          |                     |        Constraint 2        |           Not addressed                       |
|          |                     |        Constraint 3        |           Not addressed                       |
|          |                     |        Constraint 4        |           Not addressed                       |

# ADD Iteration: 3


Date: 4/12/2020

## Step 1: Refining previously created structures to fully address the remaining drivers

At this point in our design of this system, all that is left to address from a driver standpoint are the constraints. All the other drivers have been satisfied
through architectures, diagrams, or the class diagram produced in the previous two iterations.

## Step 2: Establish Iteration Goal by Selecting Drivers

Our drivers for this iteration will be all the remaining ones that we haven't looked at yet. As previously mentioned, our constraints are the only
drivers remaining, so they will be the drivers looked at during this iteration.

### Modifiability
	1. Constraint 1
	2. Constraint 2
	3. Constraint 3
	4. Constraint 4
	
## Step 3: Choose One or More Elements of the System to Refine

If we are going to go off our previous diagrams, it is pretty clear that we will be doing most of the work in the domain layer of our system.

## Step 4: Choose one or More Design Concepts That Satisfy the Selected Drivers

The best approach here is to understand that our changes need to be made in the domain layer of our existing domain diagram. The changes are not big enough
to warrant huge design decisions. Instead, the best approach here is to understand that our problems can be solved so long as we properly modify our diagram
from the previous iteration.

## Step 5: Instantitate Architetural Elements, Allocate Responsibilities, and Define Interfaces

| Design Decision & Location| Rationale |
| :---:                     |          :---: |
|   Add c files in the data_source                   |      This will allow us to load in the C programs into the Java represented classes        |
|   Make SimpleCoffeeController a subclass of AdvancedCoffeeController                   |      This will allow us to put a dispenseCoffee function in AdvancedCoffeeController, signifying that the SimpleCoffeeController doesn't have it        |
|   Use the Observer pattern for Machines                   |      This will allow machines to push their status to the CM2WManagementPlatform and not have the CM2WManagementPlatform constantly polling for their status -- to do this I will be adding a seperate class to act as the subsriber of our system        |
|    In order for our system to have no application state, we will be storing application state data in the local cache seen to the right of the Domain layer                        |        We are doing this so we don't have any application state          |
### Side Note


## Step 6: Sketch Views and Record Design Decisions

![Mod View](images/FinalDomainModel.png)

## Tactics Considered:

#### Modifiability
	1. Increase Cohesion: I ensured that all the classes and methods that were added to our preliminary class diagram were in the right place and belonged where
	they did.
	2. Reduce Coupling: This is one area that will continue to need improvement more future consideration, as there is a fair amount of coupling within our system currently.

## Step 7: Perform Analysis of Current Design and Review Iteration Goal and Achievement of Design Purpose

At this point, we have satisfied all of our drivers.


# Project Milestone 2

## Assessing Completeness

Time Spent: 2.5 hours -- spread over 2 days

### 1. Which Use Cases did the notebook's designs address? In short: how did it address them?

Design 1: This document addressed all of the use cases;  except for use case 2 and use case 5. Specifically, it attempted to address all the use cases at once by addressing the primary functionality in one iteration.
The author ended up modifying both of their reference architectures, and by also instantiating a sequence diagram to go over the workflow
of a user purchasing coffee, displaying what elements of the system would be respoonsible for what actions. Additionally, a domain model was instantiated in order
to display what nouns would be involved in solving the primary functionality, including the use cases, of the system.This was able to handle UC1, UC3, and UC4. However,
UC2 and UC5 still remained unaddressed.

Design 2: The notebook here addressed all of the use cases for the system. It did so by first selecting a service application in the first iteration and instantiating several elements
which were responsbile for achieving the use cases in the second iteration. The Client Element identified in iteration 2 could access the coffee machines and remotely check how
much coffee is left (UC2). The Client element also can make manual requests for more inventory from the supplier regarding different machines (UC3)
Additionally, the User element allows for users to be able to make coffee on machines (UC1).
The Suplier Service element is responsible
for taking requests from the supplier (UC4). The Client element also specifically mentions how it allows Admins to access data over the phone or through the
app, depending on the rules specified by the client.


### 2. Which Use Cases remain unaddressed by this notebook?

Design 1: UC2 & UC5 both remain unaddressed. I think this could have been avoided if the author tried to address each use case individually rather than just
all of the primary functionality in one iteration. In this case, it lead to use cases being left out and the system not addressing all of the initial drivers
laid out by M1.

Design 2: Although all of the use cases were properly addressed, I do have a couple comments. The author needs to fully spell out the first use case, and make
sure that the users can select different sizes of coffee (small, medium, large). This use case was addressed, but could have been clarified. Also, I think it might be
wise for the author to consolidate "Management Apps" and "Client App", since both involve the CM2W Administrator monitoring access to machines and usages.


### 3. Which Quality Attribute Scenarios did the notebook's designs address? In short: how did it address them?

Design 1: This design was able to explicity address each QAS for our initial drivers. It did so by devouting an entire interation (ADD Iteration 2, the second one -- there were 2 ADD Iteations 2s)
to the Quality Attribute scenarios. It then went on to add more conceptual classes to the domain diagram to address those issues, along with sketching a system
sequence diagram to describe how different parts of the system would react to the morning rush for coffee.

Design 2: This design addressed every QAS, except for QAS4. The author of the notebook utilized some cool patterns for the other quality attribute scenarios. For instance,
QAS1,2 and 3 were all addressed by using the Load-Balanced Cluters deplayement pattern. This pattern is utilized for it's scalability and performance. This pattern will
also utilize the many severs that were put in the architectural design. Additionally, using controllers that connect to the internet over HSDPA addressed QAS5
by "increasing data capacity and speed up transfer rates", which helps dispense coffee during the morning rush.

### 4. Which Quality Attribute Scenarios remain unaddressed by this notebook? 

Design 1: There are no quality attribute scenarios that were unaddressed in the noetbook. It was very helpful that the author of this notebook explicity labeled
the quality attribute scenarios, as it made it very easy to find and confirm. It would have been nice if the author did the same thing with the use cases.

Design 2: QAS4 was not addressed here. In order to properly address this, it probably would make sense for the author to include an additional backup-designated
server so that they could have something ready for when cell services go down.

### 5. What known Tactics from the back of our book did the notebook use to address the Quality Attribute Scenarios?

Design 1: The author did use tactics to address QAS1, QAS3*, and QAS5; although they didn't explicity name them. For QAS1, the author suggested to create
requirements for the system. For QAS3, the author suggested to store new device information in the management layer (*not sure if this is a tactic). For QAS5, the
author suggested the machine take a multi-processing state by sending information to the management platform.

Design 2: No tactics were used to address the Quality Attribute Scenarios in this design.

### 6. Did the notebook pull from external research? What did that research really add to the design? 

Design 1: Yes. There was a lot of external research from many different sources. This primarily helped with the domain models and addressing the architectural
constraints of the system. As a result, the domain models of the system were very sophisticated and well developed. Additionally, the author did a goob job
of addressing the architectual concerns of the system, and was able to identify some key considerations by using REST APIs over HTTPS. As a result of identifying
those key considerations, the author picked their reference architecture with even more supporting informatiom.

Design 2: Yes. The research provided by the external sources helped reinforce many of the design decisions that were made early on. This greatly helped the design
by backing up the decisions he made with these external sources. For instance, the load balancing algorithm that was used to address the time concerns listed in the
use cases was reinforced by research that indicated that it would be used to "make sure that requests are being processed in the quickest time possible." There are 10
external sources used throughout the document for even more supporting information.

### 7. Did the notebook compare the pros/cons of possible ideas? 

Design 1: Yes. Although not explicity stated, the author discussed both the pros and cons of each system in Step 7 of the ADD iteration1 (Analysis of current Design).
However, there could hvae been more discussion after iteration 1; as it seemed to stop after that.

Design 2: Although there was not a list-style comparison of the pros & cons of the system, the author certainly analyzed the pros and cons in the rationale
when deciding the better of two candidate designs.

### 8. Was it convenient for you to find the above information? 

Design 1: No. Although the notebook was organized into the 3 ADD iterations that were taken, along with all of the ADD steps, finding the information above
took a long time and was not convenient to find. The reason for this is simple; the author hardly ever explicity stated what he was addressing during each iteration.
Instead, the author took a general approach to the drivers and did not mention them specifically. As a result, the drivers were very hard to locate and even harder to understand
where they were being addressed.

Design 2: Yes. The notebook was well organized and neatly laid out.

## Sunny Day Test

Time Spent: 3 hours -- spread over 2 days

### 1. Write down the steps that you imagine the user/client will follow to achieve these goals.
	
Design 1: First, the user would need to open the application on their phone, and select the size and type of coffee that they wish to order.
For the client, the process would be similar. They would first need to launch the application on their phone. They would then be able to select the coffee machine of
interest and have access to inventory status of that particular machine.

Design 2: The user and client will first need to sign in to the mobile application so they are granted their respective access levels. From there, the user will
be able to select the size and type of coffee that they want from the machine of their choice. For the client, they also need to only open the mobile
application to check and see how much coffee is left as well.

### 2. What do you suppose are the Preconditions?

Design 1: First of all, both the client and the user need to have access to their phones and the CM2W application on their phones. There also needs to be cell reception
such that the coffee machine will be able to wirelessly communicate it's data back to the client and user. The user will not have access to the inventory data of the coffee machine, but the
user will be able to order coffee on a machine they have access to provided that the coffee machine has the ingredients and power necessary the user's desired coffee.
Since the design document did not mention time constraints, this could hypothetically occur at any point during the day; so long as the other preconditions I mentioned were
satisfied.

Design 2: I believe the preconditions here are that both the client and the user must have access to the CM2W mobile application on their phone, and be in a location
with acceptable cell reception. They also need to be able to successfully sign in to the application to have their respective priveleges. Additionally, the coffee machine that the user wants to make coffee from must have sufficient ingredients and power to actually make the coffee
of the user's choosing. This could happen at any time during the day; so as long as the preconditions above are satisfied, the workflow of the user ordering coffee should be able to be satisfied at any time.

### 3. What do you suppose the Postconditions are?

Design 1: I think that the postconditions here would be that the user has received their ordered cup of coffee and that inventory information has updated the system such that the
client could check how much coffee was left at that station on the mobile app if they wanted to.

Design 2: I believe the postconditions here would be that the user has ordered and recieved their coffee, and that information has beensent to the database for 
the client to check and see how much coffee is left at that station. (very similar to design 1)

## Testing Each Design

### Sequence Diagrams

Design 1: 
![Design 1 Sequence Diagram](images/Design1SeqDiagramM2.png)



Design 2:
![Design 2 Sequence Diagram](images/Design2SeqDiagramM2.png)