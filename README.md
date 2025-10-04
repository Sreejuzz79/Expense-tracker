# Expense Tracker Application

## Overview
This application is a desktop-based expense management system developed using Python's Tkinter framework. It provides a comprehensive solution for tracking personal expenses with role-based access control, ensuring user privacy while maintaining administrative oversight of system resources.

## System Architecture
The application implements a client-server architecture pattern using MySQL as the database backend and Tkinter for the graphical user interface. It employs SHA-256 encryption for password security and maintains strict separation between administrative functions and user data access.

## Core Features

### Administrative Functions
The administrative interface provides complete control over system resources without compromising user privacy. Administrators can perform the following operations:

- **User Management**: Administrators have full capability to create, edit, and delete user accounts. The system maintains referential integrity by cascading deletions to associated expense records when a user account is removed.  
- **Category Management**: The system includes a predefined set of 32 expense categories covering common expenditure types including Food, Groceries, Dining Out, Travel, Transport, Fuel, Accommodation, Utilities, Electricity, Water, Internet, Mobile, Health, Medical, Pharmacy, Insurance, Entertainment, Movies, Games, Streaming, Shopping, Clothing, Electronics, Education, Books, Courses, Fitness, Gym, Hobbies, Gifts, Charity, and Miscellaneous. Administrators can add additional categories or remove existing ones as needed.  
- **Administrative Hierarchy**: The system implements a secure administrative creation workflow. During initial setup, if no administrator exists in the database, the application displays a one-time registration option for creating the first administrator account. Once an administrator exists, this public registration option is permanently disabled. Subsequent administrator accounts can only be created by existing administrators through the administrative dashboard, ensuring controlled access to elevated privileges.  
- **Privacy Protection**: The administrative interface explicitly excludes access to individual user expense data. This design decision prioritizes user privacy by preventing administrators from viewing, editing, or analyzing personal financial information.  

### User Functions
Regular users have complete control over their personal expense tracking with the following capabilities:

- **Expense Creation**: Users can record new expenses by specifying the amount, date, category, and optional notes. The date field defaults to the current date but can be modified to record historical expenses.  
- **Expense Modification**: The update function allows users to modify any field of existing expense records, including amount, date, category, and notes.  
- **Expense Deletion**: Users can remove individual expense records with confirmation prompts to prevent accidental deletion.  
- **Category Filtering**: The expense view includes filtering capabilities allowing users to display expenses by specific categories or view all expenses simultaneously.  
- **Financial Summaries**: The system automatically calculates and displays the total expenditure for the currently filtered expense set.  
- **Data Export**: Users can export their complete expense history to Excel format using the openpyxl library. The exported file includes formatted headers, properly sized columns, individual expense records, and a calculated total. The system generates a filename automatically incorporating the username and current date.  

## Technical Specifications

### Required Dependencies
The application requires the following Python packages:

- `tkinter`: Provides the graphical user interface framework  
- `mysql-connector-python`: Enables MySQL database connectivity  
- `hashlib`: Implements password encryption using SHA-256 algorithm  
- `Pillow`: Handles image processing for optional background imagery  
- `openpyxl`: Generates Excel-formatted export files  
- `datetime`: Manages date and time operations  

### Database Structure
The application automatically initializes the database schema on first execution. The database named **expense** contains three primary tables:

- **Users Table**: Stores user account information including id (primary key), fullname (50 characters), username (50 characters, unique), email (100 characters, unique), password (255 characters for encrypted hash), and role (enumerated as admin or user).  
- **Categories Table**: Maintains expense categories with category_id (primary key) and category_name (50 characters, unique).  
- **Expenses Table**: Records individual expense transactions with id (primary key), user_id (foreign key to users table), amount (decimal with 10 digits and 2 decimal places), spent_on (date field), note (255 characters, optional), category_ids (foreign key to categories table, optional), and created_at (timestamp with automatic current timestamp).  

### Security Implementation
The application implements several security measures to protect user data:

- **Password Encryption**: All passwords are hashed using SHA-256 before storage, ensuring that plain-text passwords never exist in the database.  
- **SQL Injection Prevention**: The application exclusively uses parameterized queries with placeholder values, eliminating the possibility of SQL injection attacks.  
- **Role-Based Access Control**: The system enforces strict separation between administrative and user privileges, preventing privilege escalation and unauthorized data access.  
- **Database Validation**: All user inputs undergo validation before database operations, including format checking for email addresses, date fields, and numeric amounts.  

## Installation and Setup

### Prerequisites
Ensure MySQL Server is installed and running on the local machine. The application expects MySQL to be accessible on localhost with default port 3306. The root user should be configured without a password, or the connection parameters in the db_connection function should be modified accordingly.

### Python Environment
Install the required Python packages using pip:

```bash
pip install mysql-connector-python pillow openpyxl
The tkinter package typically comes pre-installed with Python distributions. If not available, install it through your system's package manager.

Initial Execution
Upon first execution, the application automatically performs the following initialization steps:

Creates the expense database if it does not exist

Creates all required tables with proper constraints and foreign keys

Populates the categories table with 32 predefined expense categories

Displays the main menu with the "Register as Admin" option

Configuration Options
Background Image: The application supports an optional background image for the user interface. To enable this feature, specify the full file path in the set_background_image function by replacing the empty string in the image_path variable. The image will be automatically resized to fit the window dimensions. If the specified path does not exist or the feature is not configured, the application defaults to a light gray background color.

Operation Workflow
First-Time Setup
Launch the application

Select "Register as Admin" from the main menu

Provide full name, username, email address, and password

Log in using the created administrator credentials

Optionally create additional categories through the Categories Management tab

Create initial user accounts through the Users Management tab

Daily Usage - Administrator
Log in using administrator credentials

Navigate to Users Management to add, edit, or remove user accounts

Navigate to Categories Management to modify expense categories

Create additional administrator accounts if required

Log out when administrative tasks are complete

Daily Usage - Regular User
Log in using user credentials

Navigate to Add Expense tab to record new expenses

Navigate to My Expenses tab to view, filter, update, or delete existing expenses

Use the Download to Excel button to export expense data for external analysis

Log out when expense management tasks are complete

Data Integrity and Maintenance
The application maintains data integrity through several mechanisms:

Foreign Key Constraints: The database schema enforces referential integrity between users, categories, and expenses tables, preventing orphaned records.

Cascade Deletion: When a user account is deleted, all associated expense records are automatically removed. When a category is deleted, the category reference in expense records is set to NULL rather than preventing deletion.

Duplicate Prevention: The system checks for duplicate usernames and email addresses before account creation, maintaining uniqueness constraints.

Transaction Management: All database operations are wrapped in try-except blocks with proper connection management, ensuring that connections are closed even when errors occur.

Limitations and Considerations
The current implementation has several characteristics that users should be aware of:

Local Deployment: The application is designed for single-machine deployment. Multiple users cannot access the system simultaneously from different computers without modifying the database connection architecture.

Password Recovery: The system does not include password recovery functionality. If credentials are lost, an administrator must manually reset the user's password through the Edit User interface.

Date Validation: While the system validates date format, it does not restrict entry of future dates, allowing users to create expense records for future planned expenditures.

Backup Procedures: The application does not include automated backup functionality. Database administrators should implement regular backup procedures using MySQL's native backup tools to prevent data loss.

Background Image Path: The background image feature requires an absolute file path. Users deploying the application on different systems must update this path accordingly or leave it empty to use the default background color.

File Structure
The entire application is contained within a single Python file, making deployment straightforward. The modular function structure separates concerns between database operations, user interface rendering, and business logic, facilitating maintenance and future enhancements.

Support and Troubleshooting
Database Connection Errors: Verify that MySQL Server is running and accessible. Confirm that the connection parameters in the db_connection function match your MySQL configuration.

Module Import Errors: Ensure all required Python packages are installed in the current Python environment. Use pip list to verify installed packages.

Excel Export Failures: Confirm that the openpyxl package is installed. Verify that the target directory has write permissions for the current user.

Category Display Issues: If categories do not appear in dropdown menus, verify that the initialize_categories function executed successfully during first launch. Check the MySQL console output for error messages during database initialization.

This expense tracker application provides a robust foundation for personal financial management while maintaining strong security practices and user privacy protections. The role-based architecture ensures appropriate access controls while the comprehensive feature set addresses common expense tracking requirements.
