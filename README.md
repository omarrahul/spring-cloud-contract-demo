# spring-cloud-contract-demo
demo

# Spring Cloud Contract: Online Demo

This repository demonstrates how Consumer-Driven Contract Testing with Spring Cloud Contract can prevent breaking API changes from being deployed.

The entire demo can be run online using Gitpod—no local setup required!

[![Open in Gitpod](https://gitpod.io/button/open-in-gitpod.svg)](https://gitpod.io/#https://github.com/omarrahul/spring-cloud-contract-demo)

---

### How to Run the Demo

1.  **Click the "Open in Gitpod" button above.** This will launch a complete development environment in your browser.
2.  **Wait for the Initial Build.** Gitpod will automatically run `mvn clean install` on both the `consumer-service` and `producer-service`. You will see in the terminal that both builds succeed. This confirms the producer's API currently satisfies the contract.

### The "Aha!" Moment: Introduce a Breaking Change

Now, let's simulate a developer making a change that breaks the contract.

1.  **Modify the Producer's Code:**
    *   In the file explorer on the left, navigate to `producer-service/src/main/java/com/example/producerservice/Product.java`.
    *   Change the `inStock` field from a `boolean` to a `String`:
        ```java
        // Before
        private boolean inStock;

        // After
        private String inStock;
        ```
    *   Update the constructor and getter/setter to match.

2.  **Update the Mock Data:**
    *   Navigate to `producer-service/src/test/java/com/example/producerservice/ContractTestBase.java`.
    *   Update the mocked `Product` to use a string value for `inStock`:
        ```java
        // Find this line and modify it
        when(productRepository.findById(1L)).thenReturn(
            Optional.of(new Product(1L, "Awesome Gadget", "AVAILABLE")) // Changed from 'true'
        );
        ```

3.  **Run the Build and Watch it Fail:**
    *   Open a new terminal in Gitpod (**Menu > Terminal > New Terminal**).
    *   Run the producer's build again:
        ```bash
        cd producer-service
        mvn clean install
        ```
    *   **Observe the build failure!** 💣 You will see a clear error message from Spring Cloud Contract indicating a body mismatch because it expected a boolean but got a string.

This proves that the breaking change was caught *before* the code could ever be merged or deployed.
