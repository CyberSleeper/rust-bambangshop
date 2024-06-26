# BambangShop Publisher App
Tutorial and Example for Advanced Programming 2024 - Faculty of Computer Science, Universitas Indonesia

---

## About this Project
In this repository, we have provided you a REST (REpresentational State Transfer) API project using Rocket web framework.

This project consists of four modules:
1.  `controller`: this module contains handler functions used to receive request and send responses.
    In Model-View-Controller (MVC) pattern, this is the Controller part.
2.  `model`: this module contains structs that serve as data containers.
    In MVC pattern, this is the Model part.
3.  `service`: this module contains structs with business logic methods.
    In MVC pattern, this is also the Model part.
4.  `repository`: this module contains structs that serve as databases and methods to access the databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a basic functionality that makes BambangShop work: ability to create, read, and delete `Product`s.
This repository already contains a functioning `Product` model, repository, service, and controllers that you can try right away.

As this is an Observer Design Pattern tutorial repository, you need to implement another feature: `Notification`.
This feature will notify creation, promotion, and deletion of a product, to external subscribers that are interested of a certain product type.
The subscribers are another Rocket instances, so the notification will be sent using HTTP POST request to each subscriber's `receive notification` address.

## API Documentations

You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman

After you download the Postman Collection, you can try the endpoints inside "BambangShop Publisher" folder.
This Postman collection also contains endpoints that you need to implement later on (the `Notification` feature).

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment
1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    APP_INSTANCE_ROOT_URL="http://localhost:8000"
    ```
    Here are the details of each environment variable:
    | variable              | type   | description                                                |
    |-----------------------|--------|------------------------------------------------------------|
    | APP_INSTANCE_ROOT_URL | string | URL address where this publisher instance can be accessed. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)

## Mandatory Checklists (Publisher)
-   [ ] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [ ] Commit: `Create Subscriber model struct.`
    -   [ ] Commit: `Create Notification model struct.`
    -   [ ] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [ ] Commit: `Implement add function in Subscriber repository.`
    -   [ ] Commit: `Implement list_all function in Subscriber repository.`
    -   [ ] Commit: `Implement delete function in Subscriber repository.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [ ] Commit: `Create Notification service struct skeleton.`
    -   [ ] Commit: `Implement subscribe function in Notification service.`
    -   [ ] Commit: `Implement subscribe function in Notification controller.`
    -   [ ] Commit: `Implement unsubscribe function in Notification service.`
    -   [ ] Commit: `Implement unsubscribe function in Notification controller.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [ ] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [ ] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [ ] Commit: `Implement publish function in Program service and Program controller.`
    -   [ ] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1
1. It depends on the complexity of our system. If we only want to have one type of Subscriber that will always react to changes in the same way, a single Model struct might be enough. However, if we anticipate needing different types of Subscribers in the future, or if our current Subscriber has multiple responsibilities that could be separated, it would be beneficial to define a Subscriber trait.

2. Using a DashMap allows us to quickly check for uniqueness and access elements by their unique identifiers. This is useful when we want to quickly check if a Subscriber is already in the database, or when we want to access a Subscriber by its ID. The DashMap also allows us to easily remove a Subscriber from the database, which is useful when a Subscriber wants to unsubscribe. However, the DashMap does not guarantee the order of elements, so if we need to access elements in a specific order, we might need to use a different data structure.

3. In Rust, the Singleton pattern can be implemented using lazy_static and Mutex for thread safety. However, this approach has some limitations compared to DashMap. We could replace DashMap with a Singleton pattern, it might not be the best choice if we need to support high levels of concurrency. DashMap is designed specifically for this use case and is likely to perform better.

#### Reflection Publisher-2
1. This is commonly known as separation of concerns. By separating the concerns of the Notification service and the Notification Repository, we can make our code more modular and easier to maintain. The Notification service is responsible for handling the business logic of the Notification feature, while the Notification Repository is responsible for handling the data storage and retrieval operations. This separation allows us to change the implementation of the Notification Repository without affecting the Notification service, and vice versa.

2. If we only use the model to encapsulate all responsibilites, the complexity of each model will increase significantly as we add more features. This will make the model harder to understand and maintain. By separating the concerns of the model into multiple structs, we can make each struct more focused and easier to understand. This also allows us to reuse the structs in different parts of the codebase, which can help reduce code duplication.

3. I have been using Postman to test the endpoints of app server. Postman is a powerful tool that allows us to send HTTP requests to our server and inspect the responses. It provides a user-friendly interface for creating and sending requests, as well as viewing the responses in various formats. Postman also allows us to save our requests and responses as collections, which can be shared with other team members or used for automated testing. Overall, I find Postman to be a valuable tool for testing and debugging my server-side applications.

#### Reflection Publisher-3
1. In this tutorial, we are using push model of observer pattern. We push data to the `NotificationService` whenever an event occurs. The `NotificationService` then notifies all the subscribers by sending an HTTP POST request to their respective endpoints. This approach is suitable for our use case because we want to notify the subscribers immediately when an event occurs. However, if we have a large number of subscribers or if the subscribers are slow to respond, this approach might not be the most efficient.

2. In the pull model, users have to check for updates themselves, like refreshing a webpage. This can be less efficient because they might check even when there's nothing new. However, the pull model is better with a lot of users or slow devices. It avoids overwhelming the server with constant updates.

3. Sending notifications without multi-threading works, but it's of course slower. The program waits for each notification to finish before sending the next one. This can be a big problem for a large number of subscribers or if sending each notification takes a long time.