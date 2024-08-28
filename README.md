# hotel-booking-system
Hotel Booking System

git submodule add https://github.com/Cloud2Shore/booking-orchestrator-service.git booking-orchestrator-service
git submodule add https://github.com/Cloud2Shore/user-service.git user-service
git submodule add https://github.com/Cloud2Shore/hotel-service.git hotel-service
git submodule add https://github.com/Cloud2Shore/payment-service.git payment-service
git submodule add https://github.com/Cloud2Shore/notification-service.git notification-service
git commit -m "Added microservices as submodules"
git push


------------------------
Steps to Build Modules Independently
Navigate to the Root Directory: First, open your terminal or command prompt and navigate to the root directory of your multi-module Maven project (e.g., hotel-booking-system).

Use the -pl Option with mvn Command: Use the -pl option to specify the module you want to build. Combine it with -am (also make) to build the module and its dependencies.

**mvn clean install -pl module-name -am**
module-name: Replace this with the name of the module directory you want to build, such as booking-orchestrator-service or user-service.

Examples
Example 1: Build a Single Module
To build the booking-orchestrator-service module independently:

**mvn clean install -pl booking-orchestrator-service -am**
clean install: The clean phase deletes the target directory (where compiled files are stored), and the install phase builds the project and installs the resulting artifact into the local Maven repository.
-pl booking-orchestrator-service: Specifies the module to build.
-am: Builds the specified module along with any modules it depends on.

Example 2: Build Multiple Modules
To build multiple modules independently, list them separated by commas:

**mvn clean install -pl booking-orchestrator-service,user-service -am**
This command will build both the booking-orchestrator-service and user-service modules, along with their dependencies.

Example 3: Build Without Dependencies
If you want to build a module without building its dependencies, you can omit the -am flag:

**mvn clean install -pl booking-orchestrator-service**
This will build only the booking-orchestrator-service module, assuming that its dependencies are already built or available in the local repository.

Additional Tips
Check Dependencies: Ensure that any module you are building independently has its dependencies already built and installed in the local Maven repository. Otherwise, you might get a "dependency not found" error.
Profiles: You can also use Maven profiles to customize the build for different environments or configurations.
Parallel Builds: Maven supports parallel builds using the -T option. For example, -T 4 will use four threads to build projects in parallel, which can speed up the build process if you are building multiple modules.

---------------
** Feign Client with Static URL and Load Balancing**
Declarative Feign Client with Static URL:

**Configuration:** In this approach, you configure the Feign client with a static URL, specifying the full URL of the service in the @FeignClient annotation.

**Example:**
@FeignClient(name = "user-service", url = "http://localhost:8081")
public interface UserServiceClient {
    @GetMapping("/user/authenticate")
    boolean authenticateUser(@RequestParam("userId") String userId);
}

**Declarative Nature:**
Declarative: Feign provides a declarative way to define REST calls through interfaces and annotations.
Load Balancing: Even though you can use load balancing with Feign by integrating with spring-cloud-starter-loadbalancer, it requires explicit configuration, and load balancing might not be fully dynamic if the URL is static.

**No Service Discovery:**
Static URLs: When you use static URLs, you bypass dynamic service discovery. If services scale or move, you need to manually update the URLs in your codebase.
Limited Flexibility: This approach is less flexible and doesn't fully leverage the benefits of dynamic service discovery, which means that any changes in the service instances or their locations need to be handled manually.
