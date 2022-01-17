# Invite: Event Recommendation & Ticket Search System
A web recommendation system recommends nearby events according to users' favorite events, also allows users to search events and purchase tickets.

![demo]()



## 1. Project Description 
- Frontend: an interactive web page with AJAX technology implemented with HTML, CSS, and JavaScript. This web application supports three key implementations:

- - **Auto-Search** events around users based on geolocation
   - **Favorite** events when the user likes and save them in the history records
   - Show **recommendation of events** around based on their favorite history.

- Backend: create Java servlet with RESTful APIs in Apache Tomcat to handle HTTP requests and responses

- - Utilized relational and NoSQL databases (MySQL/MongoDB) to store real business data, such as name, description, location, and price, from TicketMaster API

   - Design **content-based recommendation algorithm** for event recommendation

   - Deployed server to Amazon EC2 to handle 160 QPS tested by Apache JMeter 

     

## 2. Architecture
- 3-tier architecture
   
   * Presentation tier: HTML, CSS, JavaScript
   * Data tier: MySQL, MongoDB
   * Logic tier: Java
   
   ![Architect](img/architect.png)

## 3. Java Servlet Design
- Logic tier(Java Servlet to RPC)
   * Search
      * searchItems
      * Ticketmaster API
      * parse and clean data, saveItems
      * return response
   * History
      * get, set, delete favorite items
      * query database
      * return response
   * Recommendation
      * recommendItems
      * get favorite history
      * search similar events, sorting
      * return response
   * Login
      * GET: check if the session is logged in
      * POST: verify the user name and password, set session time and marked as logged in
      * query database to verify
      * return response
   * Logout
      * GET: invalid the session if exists and redirect to `index.html`
      * POST: the same as GET
      * return response
   * Register
      * Set a new user into users table/collection in database
      * return response

![APIs design](https://raw.githubusercontent.com/MoonSulong/EventRecommendation/master/img//APIs.png)



- TicketMasterAPI
  

- Recommendation Algorithms design
   * **Content-based Recommendation**: find categories from item profile from a user’s favorite, and recommend the similar items with same categories.

   * Present recommended items with ranking based on distance (geolocation of users)

     

## 4. Database Implementation
- MySQL
   * **users** - store user information.
   * **items** - store item information.
   * **category** - store item-category relationship
   * **history** - store user favorite history

![mysql](https://raw.githubusercontent.com/MoonSulong/EventRecommendation/master/img/mysql.png)



- MongoDB
   * **users** - store user information and favorite history. = (users + history)
   
   * **items** - store item information and item-category relationship. = (items + category)
   
   * **logs** – store log information
   
     

## 5. Design pattern
   * **Builder pattern**: `Item.java`
      
         * When convert events from TicketMasterAPI to java Items, use builder pattern to freely add fields.
         * **Factory pattern**: `ExternalAPIFactory.java`, `DBConnectionFactory.java`
      
      * `ExternalAPIFactory.java`: upport multiple functions like recommendations of event, restaurant, news, jobs, etc., Just link to different public API. 
      * `DBConnectionFactory.java`: support multiple database like MySQL and MongoDB. Improve extension ability.
      
   * **Singleton pattern**: `MySQLConnection.java`, `MongoDBConnection.java`
      
      * Only create one specific instance of the database, and the class can control the instance itself and give global access to the outer class.
