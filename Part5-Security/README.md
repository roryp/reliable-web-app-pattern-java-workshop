# Security

Welcome to the "Security" section of our workshop. In this part, we will delve into Proseware's Cloud Security Architecture. We'll explore how Proseware leverages advanced cloud technologies and services from Microsoft Azure and Microsoft Entra ID to secure their web applications against various cyber threats.

1. Modifying Security Configurations: Adjusting the `GlobalSecurityConfig.java` to control access, allowing public content visibility while securing other parts of the application.
2. Updating the Playlist Entity: Implementing a public/private toggle in `Playlist.java`` to manage playlist visibility.
3. Creating a "Sonos" Account: Developing a method in UserService.java to add a specialized user with specific permissions.
4. Deploying with Azure Developer CLI: Utilizing Azure's tools to deploy and make the application live.

## Steps

### Step 1: Modify Security Configurations
**File:** `GlobalSecurityConfig.java`
1. Open `GlobalSecurityConfig.java` in your IDE.
2. Locate the `WebSecurityConfiguration` class.
3. Add the following method to configure HTTP security:

   ```java
   @Override
   protected void configure(HttpSecurity http) throws Exception {
       http
           .authorizeRequests()
           .antMatchers("/public/**").permitAll()
           .anyRequest().authenticated()
           // existing configuration
   }
   ```

### Step 2: Update Playlist Entity
**File:** `Playlist.java`
1. Open `Playlist.java`.
2. Find the `shared` field. If not present, add `private boolean shared;`.
3. Add getter and setter methods for the `shared` field:

   ```java
   public boolean isShared() {
       return shared;
   }

   public void setShared(boolean shared) {
       this.shared = shared;
   }
   ```

### Step 3: Create "Sonos" Account
1. Identify the user service file, typically `UserService.java`.
2. Create a method to add a new user:

   ```java
   public void createSonosAccount() {
       User sonosUser = new User("sonos", "password"); // Use a secure password
       // Set necessary permissions
       userRepository.save(sonosUser);
   }
   ```
3. Invoke this method at an appropriate place in your application startup or through an admin panel.

### Step 4: Deploy the App with Azure Developer CLI
1. Deploy the application: `azd up`
2. Once deployed, access the application URL provided by Azure.

## Conclusion
You have now enhanced your application's security and successfully deployed it using Azure Developer CLI.

Next, we will delve into operational excellence in cloud applications. Please proceed to [Part 6 - Operational Excellence](../Part6-Operational-Excellence/README.md) for more information.