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
-   [x] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [x] Commit: `Create Subscriber model struct.`
    -   [x] Commit: `Create Notification model struct.`
    -   [x] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [x] Commit: `Implement add function in Subscriber repository.`
    -   [x] Commit: `Implement list_all function in Subscriber repository.`
    -   [x] Commit: `Implement delete function in Subscriber repository.`
    -   [x] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [x] Commit: `Create Notification service struct skeleton.`
    -   [x] Commit: `Implement subscribe function in Notification service.`
    -   [x] Commit: `Implement subscribe function in Notification controller.`
    -   [x] Commit: `Implement unsubscribe function in Notification service.`
    -   [x] Commit: `Implement unsubscribe function in Notification controller.`
    -   [x] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [x] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [x] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [x] Commit: `Implement publish function in Program service and Program controller.`
    -   [x] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [x] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1

1. **Subscriber as an Interface (Trait in Rust)** :
   <br>In the Observer pattern, `Subscriber` is often defined as an interface (or trait in Rust). However, in this project:
   <br>
   - **The `Subscriber` struct only needs to store a `url` and `name`**, with no varying behaviors.
   - **Rust's type system ensures consistency** without requiring polymorphism.
   - **A trait would add unnecessary complexity** unless we need different subscriber types in the future.
   
   Conclusion : **A single Subscriber struct is sufficient.**


2. **Using `Vec` vs. `DashMap` for Storing Subscribers** :
   <br>**The `id` in `Program` and `url` in `Subscriber` must be unique.**
   <br>

   - **A `Vec` requires linear search (O(n)) for lookups**, which is inefficient for large datasets.
   - **`DashMap` provides constant-time (O(1)) operations and is thread-safe**, making it ideal for concurrent access.
   
   Conclusion : **`DashMap` is more efficient and necessary for this use case.**


3. **DashMap vs. Singleton Pattern** :
   <br>**The `SUBSCRIBERS` database uses `DashMap` for thread safety.**
   <br>

   - **The Singleton pattern ensures a single instance** but does not guarantee thread safety without additional synchronization.
   - **`DashMap` is designed for concurrent access and handles thread safety internally**, simplifying implementation.
   
   Conclusion : **`DashMap` is a better choice than Singleton for managing `SUBSCRIBERS`.**

#### Reflection Publisher-2

1. **Why Separate "Service" and "Repository" from the Model in MVC?**

    Separating Service and Repository from the Model ensures adherence to design principles like Separation of Concerns and Single Responsibility . The Repository handles data storage, abstracting database interactions, while the Service manages business logic, orchestrating operations between models. This separation makes the codebase modular, reusable, and easier to maintain. For instance, switching databases or modifying business logic becomes simpler without affecting other layers. Without this separation, the Model would become bloated, handling both data and logic, leading to tightly coupled and hard-to-maintain code.

    Additionally, separating these layers improves testability and scalability. Unit tests can focus on individual components (e.g., testing repositories for CRUD operations or services for business rules) without needing to mock the entire system. As the application grows, this modular structure ensures that new features can be added without disrupting existing functionality, making it ideal for large-scale projects.


2. **What Happens If We Only Use the Model?**
   
    If we rely solely on the Model , it would handle both data storage and business logic, leading to tightly coupled and complex code. For example, the Program model might directly interact with the Subscriber model to notify users, and the Notification model might fetch product details from the Program model. This tight coupling increases code duplication and makes it harder to modify or extend individual models without affecting others. Testing also becomes challenging, as mocking database interactions within the Model is cumbersome.

    Moreover, as the application scales, the Models would grow bloated, containing redundant logic and becoming difficult to debug or refactor. By introducing Service and Repository layers, we decouple responsibilities, reducing complexity and improving maintainability. Each layer focuses on a specific task, ensuring cleaner, more organized code.


3. **Exploring Postman and Its Features**

    Postman has been an invaluable tool for testing REST APIs in this project. It simplifies sending HTTP requests, allowing me to quickly test endpoints like /subscribe and /unsubscribe. Features like environment variables make it easy to manage configurations (e.g., base URLs), while collections help organize related requests for better workflow management. Automated scripts in Postman are particularly useful for validating responses and ensuring the API behaves as expected.

    For group projects, Postman’s collaboration features, such as shared collections and workspaces, streamline teamwork by ensuring everyone uses the same test cases. I’m also interested in advanced features like mock servers for simulating API responses during frontend development and API documentation for communicating endpoint details with stakeholders. These tools will be helpful for future software engineering projects, enabling efficient API development and testing.

#### Reflection Publisher-3

1. **Which Variation of Observer Pattern Do We Use?**

    We use the Push model of the Observer Pattern, where the publisher sends notifications directly to subscribers via HTTP POST requests. This approach ensures real-time updates and simplifies the communication flow, as subscribers don’t need to request data manually. The Push model works well for this case because it avoids the inefficiency of polling and ensures timely delivery of product-related events (e.g., creation, promotion, deletion).

    However, the Push model assumes that subscribers are always available. If a subscriber is unreachable, notifications may fail, requiring additional error handling or retry mechanisms.


2. **Advantages and Disadvantages of Using the Pull Model**
   
    The Pull model would require subscribers to periodically fetch updates from the publisher. While this gives subscribers control over when to check for updates, it introduces inefficiency, as they may repeatedly poll without receiving new data. It also increases latency, as updates are not delivered in real-time.

    For this tutorial, the Pull model would complicate the system by shifting the responsibility of tracking changes to subscribers. They would need to maintain state or timestamps to identify new updates, making the system less responsive and harder to manage compared to the Push model.


3. **What Happens Without Multi-Threading in the Notification Process?**

    Without multi-threading, notifications would be sent sequentially, one subscriber at a time. This would slow down the process, especially if one subscriber’s URL is slow or unresponsive, blocking updates to others. As the number of subscribers grows, the system would become a bottleneck, reducing scalability and responsiveness.

    Multi-threading allows notifications to be sent concurrently, improving performance and ensuring timely updates. Removing it would simplify the code but severely impact the system’s ability to handle concurrent requests efficiently, making it unsuitable for real-world applications.