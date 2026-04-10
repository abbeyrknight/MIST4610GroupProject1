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

## Data Dictionary: 

## Queries: 
1. Which customers have not signed their liability waiver?
Managerial justification: WES needs to ensure all participants have signed waivers before trips for liability protection.
SELECT c.customer_ID, c.customer_F_name, c.customer_L_Name, r.Registration_status
FROM Customer c JOIN Registration r ON c.customer_ID = r.Customer_customer_ID
WHERE r.Registration_waiver_Signed = 'No'
ORDER BY c.customer_L_Name;
<img width="996" height="372" alt="image" src="https://github.com/user-attachments/assets/520a69a0-0f20-4cd0-ab02-e6e1c2e880f8" />

2. Which guests are in the database and who is their sponsor?
Managerial justification: Helps WES verify that every guest has a valid
university-affiliated sponsor.
SELECT customer_ID, customer_F_name, customer_L_Name, customer_Sponsor_ID
FROM Customer
WHERE customer_Type = 'Guest'
ORDER BY customer_L_Name, customer_F_name;
<img width="748" height="336" alt="image" src="https://github.com/user-attachments/assets/5437170d-c830-4d5d-b5dc-392d6d3d901c" />

3. Which registrations are currently waitlisted?
Managerial justification: Helps WES monitor demand and decide whether to add
more trip capacity.
SELECT Registration_ID, Customer_customer_ID, Registration_status, Registration_waiver_Signed
FROM Registration
WHERE Registration_status = 'waitlisted'
ORDER BY Registration_ID;
<img width="776" height="312" alt="image" src="https://github.com/user-attachments/assets/7afc9364-8732-44ad-87fb-96bd93649e30" />

4. How many customers are there of each customer type?
Managerial justification: Helps WES understand its membership composition to tailor
outreach and pricing strategies.
SELECT customer_Type, COUNT(*) AS total_customers
FROM Customer
GROUP BY customer_Type
ORDER BY total_customers DESC;
<img width="694" height="414" alt="image" src="https://github.com/user-attachments/assets/169e5bf7-47f7-45da-b73b-7c62df03af44" />

Complex Query 1: How many registrations are there for each registration status?
Managerial justification: Helps WES track how many participants are confirmed,
waitlisted, or cancelled.
SELECT Registration_status, COUNT(*) AS total_registrations
FROM Registration
GROUP BY Registration_status
ORDER BY total_registrations DESC;
<img width="480" height="264" alt="image" src="https://github.com/user-attachments/assets/72157768-9cd7-4eca-9ab2-fb61de6387c5" />

Complex Query 2: Which customers have more than one registration?
Managerial justification: Helps WES identify highly engaged customers who participate in multiple trips.
SELECT c.customer_ID, c.customer_F_name, c.customer_L_Name, COUNT(r.Registration_ID) AS total_registrations
FROM Customer c
JOIN Registration r ON c.customer_ID = r.Customer_customer_ID
GROUP BY c.customer_ID, c.customer_F_name, c.customer_L_Name
HAVING COUNT(r.Registration_ID) > 1
ORDER BY total_registrations DESC;
<img width="660" height="394" alt="image" src="https://github.com/user-attachments/assets/fd7d2970-492d-4571-b19d-bd26b4fd490a" />

Complex Query 3:  How many registrations does each customer type have?
Managerial justification: Helps WES understand which types of customers are participating most often.
SELECT c.customer_Type, COUNT(r.Registration_ID) AS total_registrations
FROM Customer c
JOIN Registration r ON c.customer_ID = r.Customer_customer_ID
GROUP BY c.customer_Type
ORDER BY total_registrations DESC;
<img width="638" height="334" alt="image" src="https://github.com/user-attachments/assets/d5e8de8b-73df-40dc-aa55-196d6a0547c9" />

Complex Query 4: Which customers have signed all of their waivers?
Managerial justification: Helps WES identify participants who are fully cleared for
all trips they registered for.
SELECT c.customer_ID, c.customer_F_name, c.customer_L_Name,
COUNT(r.Registration_ID) AS total_registrations
FROM Customer c
JOIN Registration r ON c.customer_ID = r.Customer_customer_ID
GROUP BY c.customer_ID, c.customer_F_name, c.customer_L_Name
HAVING COUNT(r.Registration_ID) = SUM(CASE WHEN r.Registration_waiver_Signed = 'Yes' THEN 1 ELSE 0 END)
ORDER BY c.customer_L_Name, c.customer_F_name;
<img width="548" height="638" alt="image" src="https://github.com/user-attachments/assets/4ad1cad4-cf20-4734-a2f0-75035fd19e28" />
Complex Query 5: Which customers have at least one cancelled registration?
Managerial justification: Helps WES identify cancellation patterns and follow up
with customers if needed.
SELECT DISTINCT c.customer_ID, c.customer_F_name, c.customer_L_Name, c.customer_Type
FROM Customer c
JOIN Registration r ON c.customer_ID = r.Customer_customer_ID
WHERE r.Registration_status = 'cancelled'
ORDER BY c.customer_L_Name, c.customer_F_name;
<img width="992" height="398" alt="image" src="https://github.com/user-attachments/assets/f6e39ce7-b1cf-4f77-9dfc-83943f628604" />

Complex Query 6: Which customer types have the highest percentage of unsigned waivers?
Managerial justification: Helps WES identify which customer groups may need
more waiver reminders.
SELECT c.customer_Type, COUNT(r.Registration_ID) AS total_registrations, SUM(CASE WHEN r.Registration_waiver_Signed = 'No' THEN 1 ELSE 0 END) AS unsigned_waivers,
 CAST(100.0 * SUM(CASE WHEN r.Registration_waiver_Signed = 'No' THEN 1 ELSE
0 END) / COUNT(r.Registration_ID) AS DECIMAL(5,2)) AS percent_unsigned
FROM Customer c
JOIN Registration r ON c.customer_ID = r.Customer_customer_ID
GROUP BY c.customer_Type
ORDER BY percent_unsigned DESC;
<img width="1002" height="400" alt="image" src="https://github.com/user-attachments/assets/4bf38868-963e-4e8d-bbea-08d729995290" />
## Database information: 
Name of the Database: 
