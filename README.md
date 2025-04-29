# express-login-kit
Handles auth, signup, and validation

This project is a login kit that handles authentication, signup, and validation for websites and apps. It’s meant to intergrate easily so you can quickly add login features to an app or website without having to build from scratch. 

I built this as an add on for my Software Engineering course. We ended up not needing it but I decided to finish devloping it. Below is an explanantion of everything it does, how to set it up, and ideas that could improve its functioanlity. 

### What this kit gives you

 - Easy integration with Express.js backend
 - SQLite database for secure, lightweight storage
 - Secure password handling (hashed and salted)
 - Token-based user authentication
 - User roles and permissions
 - Email-based account verification
 - Password reset functionality
 - Admin panel for user management
 - Basic rate limiting for security
 - Basic logging capabilities
 - Optional Two-Factor Authentication (2FA)




### Tech stack : 
 Backend built out with express js and SQLite

### Database:

 Holds the users account information and stores AUTH Tokens 

 ``` 
 CREATE TABLE users (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  email TEXT UNIQUE NOT NULL,
  password TEXT NOT NULL,
  role TEXT NOT NULL,
  verification_token TEXT,
  two_factor_enabled INTEGER DEFAULT 0,
  two_factor_secret TEXT
 );

 CREATE TABLE password_resets (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  user_id INTEGER NOT NULL,
  reset_token TEXT NOT NULL,
  created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY (user_id) REFERENCES users(id)
 );

 CREATE TABLE login_attempts (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  user_id INTEGER,
  ip_address TEXT,
  success INTEGER NOT NULL,
  attempt_time DATETIME DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY (user_id) REFERENCES users(id)
 );

 CREATE TABLE logs (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  user_id INTEGER,
  action TEXT NOT NULL,
  details TEXT,
  logged_at DATETIME DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY (user_id) REFERENCES users(id)
 );

 CREATE TABLE roles (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  role_name TEXT UNIQUE NOT NULL,
  permissions TEXT
 );
```

### Secure Password Handling
 **Important:** You must follow the steps below to ensure security.

 salted hashes to protect user passwords. To make this work, you need to create a salt list. We've included an example that selects a salt based on a hash of the user's email, so it consistently picks the same salt every time.

 The list of stored hasehes is in ---- you can update it but that will requrie resets on all users that are affected (Im assuming youd only ever need to reset all for secuurity) Below in admin you will find the process for password resets 

### Authentication
 The backend generates secure cookies and things to auth users 

 Cookies have been secured with HttpOnly, and .... 
 ***Important*** Must be https 



### Role Based access control 
 Utlaizing the Auth tokens you can make calls to the DB to verify that a user is authorized to acive what they are 
 This is useful to prevent users from performing things stuch as privalige escalation

 Here is an example of the limiting a control to only admin 

 ```
Code with comments
 ```
 At the start when calling the function for access it ensures that the admin is an admin by checkung the secure token

### Account Email Verfification 
 For the software it is built out to currently use googles smp--- but you can easily modify it to work with mailchip? and mail gunner or other services. This is currently located in  ------ youll need to locate the function blank 

 ***Show the function here with comments 

 Its setup to use that secure preplacer for secure thigns 
 
### Password Reset Functionality 

### Admin panel for managment 

### Basic Rate Limiting 

### Detailed Logging 

### Detailed Logging

The `logs` table tracks important user and system actions for security and auditing purposes.

Each log entry records:
- **user_id** (optional, if tied to a user)
- **action** (what happened, like "password_reset" or "login_failed")
- **details** (extra information if needed)
- **logged_at** (timestamp when the event occurred)

This allows admins to monitor user behavior, spot suspicious activity, and maintain a record of important changes in the system. You can easily expand this to track more actions as the project grows.

Action can be written in as 






Additional Improvemnt Ideas: Add additional tables for improved security. Add user flags like Mailing list or TOS agreement(Check for agreement of most recent version upon login)


##### Setting Up Email

The outline in this doc is currently a shell and needs logic implemented. For my use, it was primarily for site testing, and Google’s free tiered SMTP service was enough for me.
There are other services and APIs you can utilize for this, such as SendGrid, Mailgun, or Amazon SES. *I have not tested any of these additional serviced 

### Secuirty Features 
### Enabling 2 fac auth (optional)
 






It can be modified to have additional features like username, number, a friends list. This is just a basic neccisary 
CREATE TABLE users (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  email TEXT UNIQUE NOT NULL,
  password TEXT NOT NULL,
  role TEXT NOT NULL,
  verification_token TEXT
);

id automativally genenrated, incremented.
email is the primary used source for the identification. 
password: holds the hased and salted password
role currently holds the values pending, pleb, admin, and banned. you can add new roles ontop of these for tiered access. (I wonder the best way to do modular). 
verfifical_token is valid for when a password is needing or requested to be reset

The Longer you think the harder the problem becomes. Its silly cause solutions are very straight forward and can be easily addressed. 

### Email Verification (Account Creation)
I labled this as account creation because there is no current option to change or update an email. so this is currently just in use for creation

to create the first the first account youll need to launch sqlite3 and add in the details manually

Graph of the Flow 
Account Management: 


*Mermaid code placeholder*
Creating an Account 
graph TD;
  A[User Registers with email and password] --> B{Check db for the email}
  B -- Yes --> C{Role is pending?}
  C -- Yes --> D[Check token age]
  D -- Expired --> E[Send new verification email\\nShow generic message]
  D -- Still valid --> F[Tell user to wait before resending]
  C -- No --> G[Already registered\\nShow generic message]
  B -- No --> H[Hash password and create user in 'pending' status]
  H --> I[Send verification email\\nShow generic message]
  I -- User Validated Email --> J[Account set to active]



Resetting Passwords:



Update Email: 


### Aditional Considerations 
