# Team A1 MIST4610 Group Project 1 
## Team Name: 
Section 21484 GroupA1 
## Team Members: 
1. Abbey Knight - Group Leader
2. Elesia Deli'Organo - Conceptual Modeler
3. Will Holcombe - Database Designer
4. Andrew Bingham - Data Wrangler / SQL Writer 
## Case Description - The Wilderness Exploration Society 
The task at hand is to model and build a relational database for Wilderness Exploration Society (WES) at Peachtree State University. The central entity in the model is the customer as they are required in the two parallel subsystems of trips and rentals. The database will support the WES’s day to day operations such as managing customers, coordinating outdoor trips, and handling equipment rentals. The Wilderness Exploration Society offers services to university affiliated members and guests with a variety of trips and rental equipment that must be efficiently tracked. We are interested in accurately modeling these relationships and want to capture relevant details about customers, trips, equipment, staff, registrations, rental activity, etc. Overall, we are interested in creating a system to store both current and historical data that provides valuable insights into WES’s operations and resource usage. 
## Data Model 
Explanation of Data Model: 

Our model is based on the structure of the Wilderness Exploration Society (WES) at Peachtree State University, which manages both trip registrations and equipment rentals. The Customer entity is representative of all customers of the Wilderness Exploration Society. Customers can either be University-affiliated or guests of a University-affiliated customer. Each guest must be sponsored by one member, while one member can sponsor multiple guests. This is implemented through a recursive relationship in the Customer table. 

Customers can participate in outdoor trips, which are defined in the Trip Type entity. Each trip type includes attributes such as trip name, difficulty level, fee, and length. Some trips require prerequisites, which are modeled using the Trip Prerequisite entity. This creates a many-to-many relationship where one trip type can require multiple other trip types, and also serve as a prerequisite for many. 

Each trip type can be offered multiple times throughout the year, which is represented by the Scheduled Trip entity. This entity includes the specific date of the trip and connects back to Trip Type through a one-to-many relationship. Additionally, each scheduled trip is led by WES staff, so there is a one-to-many relationship between the Scheduled Trip and the Staff entity, which stores staff member information. Staff development is tracked using the Staff Training entity. 

Customers register for scheduled trips through the Registration entity, which connects Customer and Scheduled Trip, and includes things such as the registration status, waiver, and constraint. Each customer can only have one registration per scheduled trip, enforcing a one-to-many relationship from both Customer and Scheduled Trip into Registration.

On the equipment side of the Wilderness Exploration Society, there are two entities: Equipment Item and Equipment Type. Equipment Item represents individual items with condition ratings, and Equipment Type defines categories and rental rates by customer group. The Equipment Maintenance entity tracks repair issues, costs, and expected return dates. 

Equipment rentals are managed through the Rental Agreement entity, which links customers and staff to a rental transaction. The Rental Item entity is an associative table that records individual items, expected, and actual return dates within each agreement. 

Trips and Equipment rentals are handled independently, so customers can participate in either or both. Lastly, the Customer Training, Equipment Maintenance, and Staff Training are extensions that we added. These enhance the model by tracking training completion for customers and staff and keeping a record of repairs and maintenance performed on equipment, without affecting the core entities. 

<img width="1000" height="1402" alt="image" src="https://github.com/user-attachments/assets/c3a94ad8-72ff-4445-9514-3c9f1d848ec1" />

