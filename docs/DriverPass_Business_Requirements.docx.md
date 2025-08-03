# CS 255 Business Requirements Document

## System Components and Design

### Purpose

The purpose of this project is to develop a comprehensive web-based training system for DriverPass, a company founded by Liam that aims to reduce the current 65% failure rate at DMV driving tests. After analyzing the interview transcript, I've identified that DriverPass wants to create an integrated platform that combines online practice exams with on-the-road training sessions.

The client, DriverPass, needs a system that allows students to:
- Take online practice tests that mirror actual DMV exams
- Schedule on-the-road training sessions with certified instructors
- Track their progress through various training modules
- Access study materials from any device with internet connectivity

### System Background

Through my analysis of the DriverPass interview, I've identified several key problems that need to be addressed:

The primary issue is that over 65% of students fail their driving tests because they rely solely on memorizing previous test questions rather than developing actual driving skills. Currently, there are very few comprehensive tools available that combine theoretical knowledge with practical driving experience. This gap in the market presents an opportunity for DriverPass to create a solution that addresses both aspects of driver training.

The system components I've identified as necessary include:
- An online testing platform that provides practice exams similar to actual DMV tests
- A scheduling system that manages appointments for on-the-road training
- User account management supporting multiple roles (Owner, IT Officer, Secretary, Customers, and Instructors)
- A package management system for three different training tiers
- Integration with DMV systems to ensure training materials stay current
- Activity tracking and reporting capabilities for compliance and monitoring
- Secure payment processing for customer transactions

### Objectives and Goals

Based on my understanding of the client's needs, the completed system should achieve the following objectives:

The system must enable customers to take online classes and practice tests at their convenience. This includes providing a user-friendly interface where students can access training materials, take timed practice exams, and review their results to identify areas needing improvement.

For scheduling, the system needs to allow customers to book on-the-road training sessions in 2-hour blocks. The scheduling engine must prevent double-booking of instructors and vehicles while giving customers flexibility in choosing convenient time slots.

User progress tracking is essential for helping students understand their readiness for the actual DMV test. The system should maintain detailed records of test scores, completion status, and instructor feedback from driving sessions.

Different access levels must be implemented for various user types. The Owner (Liam) needs full system access with reporting capabilities, the IT Officer (Ian) requires administrative privileges for user management, the Secretary needs appointment booking capabilities, and Customers need self-service access to their training materials and schedules.

The system must generate comprehensive activity reports showing all modifications, including who made changes and when. This audit trail is crucial for accountability and troubleshooting.

Integration with DMV systems ensures that practice tests and training materials reflect current regulations and policies. The system should automatically sync with DMV updates and notify users of important changes.

Finally, the system must support three training packages while allowing the owner to enable or disable packages without requiring code changes. This flexibility is important for business adaptability.

## Requirements

### Nonfunctional Requirements

#### Performance Requirements

After reviewing the interview transcript, I've determined the following performance requirements:

The system must be web-based and run on cloud infrastructure to ensure accessibility and reliability. Liam specifically mentioned not wanting to deal with backup and security at the infrastructure level, making cloud hosting the ideal solution.

The system should handle online access from any computer or mobile device, providing a responsive interface that adapts to different screen sizes. While the interview didn't specify exact speed requirements, modern web applications should aim for page load times under 3 seconds and API response times under 500 milliseconds for optimal user experience.

For offline capabilities, the system should allow users to download reports and some training materials for offline study. However, all data modifications must occur online to prevent synchronization conflicts and maintain data integrity.

The DMV content synchronization should occur daily, preferably during off-peak hours, to ensure students always have access to current information without impacting system performance during busy periods.

#### Platform Constraints

Based on the technical discussions in the interview, the following platform constraints apply:

The system must run on modern web browsers including Chrome, Firefox, Safari, and Edge. This browser-based approach eliminates the need for native application development while ensuring broad accessibility.

The backend requires a robust database system to store user information, test results, scheduling data, and activity logs. A relational database like PostgreSQL or MySQL would be appropriate for maintaining data relationships and ensuring transactional integrity.

The system should work seamlessly on both desktop computers and mobile devices. This requires responsive design principles and possibly a progressive web app approach for mobile users.

Cloud hosting is required, with the provider handling infrastructure concerns like backup, security patches, and scalability. Services like AWS, Azure, or Google Cloud would meet these requirements.

#### Accuracy and Precision

To ensure system reliability and data integrity, the following accuracy requirements must be met:

Each user must have unique account credentials, with email addresses serving as primary identifiers. The system should validate email formats and prevent duplicate registrations.

The system must track every data modification with precise timestamps and user attribution. This includes logging who created appointments, who modified them, and who canceled them, providing a complete audit trail.

The IT administrator needs real-time visibility into system status, including notifications when users request password resets or when system errors occur that require intervention.

For scheduling accuracy, the system must prevent double-booking through real-time availability checking. When a customer or secretary attempts to book an appointment, the system should immediately verify instructor and vehicle availability before confirming.

#### Adaptability

The system must be designed with future growth and changes in mind:

User management should be possible without code modifications. Ian, as the IT Officer, needs to add, modify, or remove user accounts through an administrative interface without requiring developer assistance.

The system must support password reset functionality, both through self-service for customers who forget their passwords and through administrative reset by the IT Officer.

Package management requires flexibility. While adding entirely new packages will require developer involvement in version 1, Liam needs the ability to enable or disable existing packages based on business needs.

The system should be designed to accommodate future enhancements like additional training modules, new package types, or expanded geographic coverage without major architectural changes.

#### Security

Security is paramount for protecting user data and payment information:

All users must authenticate before accessing the system, with passwords meeting complexity requirements (minimum length, mixed characters, etc.). The system should implement session timeouts to prevent unauthorized access on shared computers.

Access control must be role-based, with the IT Officer able to immediately revoke access for terminated employees. This includes invalidating active sessions and preventing future logins.

Customer payment information, including credit card numbers, expiration dates, and security codes, must be encrypted both in transit and at rest. The system should comply with PCI DSS standards for payment card security.

All connections between clients and servers must use HTTPS encryption to prevent data interception. This includes web traffic, API calls, and any mobile device communications.

The system should implement automated security measures like account lockout after multiple failed login attempts and notification of suspicious activities to administrators.

### Functional Requirements

Based on my analysis of the DriverPass interview, the system shall implement the following functional requirements:

**Authentication and Access Control**
- The system shall validate user credentials during login, checking username/password combinations against the secure database
- The system shall implement role-based permissions, ensuring each user type (Owner, IT Officer, Secretary, Customer, Instructor) only accesses appropriate features
- The system shall provide automated password reset functionality via email verification

**Scheduling and Appointment Management**
- The system shall allow customers to schedule driving lesson appointments online through a user-friendly calendar interface
- The system shall enable the secretary to book appointments via phone by entering customer information into the system
- The system shall enforce unique constraints on instructor/vehicle/timeslot combinations to prevent double-booking
- The system shall map each appointment to a specific customer, instructor, vehicle, and 2-hour time slot
- The system shall allow customers to modify or cancel their appointments with appropriate notice

**Package and Training Management**
- The system shall support three training packages: Package 1 (6 hours driving), Package 2 (8 hours driving + in-person class), and Package 3 (12 hours driving + in-person class + online access)
- The system shall track remaining hours for each customer based on completed lessons
- The system shall allow the owner to enable or disable package availability without code changes

**Progress and Performance Tracking**
- The system shall log all test attempts with scores, time taken, and pass/fail status
- The system shall display a progress grid showing test status: not taken, in progress, failed, or passed
- The system shall store instructor notes and comments for each completed driving lesson
- The system shall calculate and display overall readiness scores for students

**External Integration and Updates**
- The system shall poll the DMV API daily for regulation and policy updates
- The system shall notify users when DMV policies change that might affect their training
- The system shall maintain version history of DMV content for compliance purposes

**Reporting and Data Export**
- The system shall generate activity reports filtered by date, user, or action type
- The system shall log all data modifications with user identification and timestamps
- The system shall export data to Excel format for offline analysis
- The system shall provide downloadable reports for management review

**Payment and Customer Management**
- The system shall securely store customer contact information including name, address, phone, and email
- The system shall process and store payment information in encrypted format
- The system shall track payment status for each customer's package
- The system shall generate receipts for completed transactions

### User Interface

The user interface must accommodate different user types with varying technical abilities and access needs:

**User Roles and Access Levels:**

**Owner (Liam)** - As the business owner, Liam requires:
- Dashboard showing key business metrics (active students, completion rates, revenue)
- Full access to all system reports and data exports
- Ability to download data for offline analysis in Excel
- Package management controls to enable/disable offerings
- Complete visibility into all system activities and user actions

**IT Officer (Ian)** - As the technical administrator, Ian needs:
- User management interface for creating, modifying, and deleting accounts
- Password reset capabilities for any user
- Access control panel to immediately revoke permissions
- System health monitoring and error logs
- Ability to manage user roles and permissions

**Secretary** - The front-desk staff requires:
- Simple appointment booking interface for phone-based scheduling
- Customer search and information entry screens
- Calendar view showing instructor and vehicle availability
- Ability to modify or cancel appointments on behalf of customers

**Customers** - Students using the system need:
- Self-registration portal with intuitive forms
- Personal dashboard showing package details and progress
- Online appointment scheduling with available time slots
- Test-taking interface with timer and progress indicators
- Access to training materials and practice resources

**Instructors** - Driving instructors need:
- Daily schedule view showing assigned students
- Interface for entering lesson notes and feedback
- Access to student progress and previous lesson history

**Interface Specifications:**

The platform must be web-based and accessible through standard browsers. The responsive design should automatically adjust layouts for desktop, tablet, and mobile devices.

Multiple entry points are required: customers primarily use web browsers for self-service, while the secretary handles phone-based bookings through a dedicated interface. The system should support concurrent users without performance degradation.

Based on Liam's mockup, specific interface components include:

**Online Test Progress Display:**
The test tracking interface should show a table with columns for:
- Test name (e.g., "Traffic Signs," "Road Rules")
- Time taken to complete each test
- Score achieved (percentage and points)
- Current status using clear indicators

**Driver Notes Section:**
The instructor feedback interface requires:
- Date and time of each lesson
- Instructor name
- Detailed comments about student performance
- Areas identified for improvement
- Recommendations for future lessons

Additional interface requirements include:
- Clear navigation menus organized by user role
- Consistent branding with DriverPass colors and logo
- Accessibility features for users with disabilities
- Help documentation and tooltips for complex features
- Session timeout warnings to protect user data

## Assumptions

In developing this business requirements document, I've made several assumptions based on the interview transcript and standard industry practices:

**Technical Infrastructure:**
- Users have reliable internet access for online features
- Modern web browsers are available to all users (Chrome, Firefox, Safari, Edge)
- The DMV will provide some form of API or data feed for regulation updates
- Cloud service providers will maintain 99.9% uptime SLAs
- Payment processing will integrate with established providers like Stripe or PayPal

**Business Operations:**
- DriverPass has already secured or will secure 10 vehicles and qualified instructors
- The company has obtained necessary licensing to use DMV practice content
- A business phone line is available for secretary-based scheduling
- Physical office space exists for in-person classes (Packages 2 and 3)

**User Behavior:**
- Customers have valid email addresses for account creation
- Students are comfortable using web-based learning platforms
- Instructors can provide feedback through web interfaces
- Customers will provide accurate information during registration

**Regulatory Compliance:**
- DMV content can be legally reproduced for training purposes
- Student data retention follows state and federal guidelines
- Payment processing will meet PCI compliance requirements
- The system can adapt to changing DMV requirements

## Limitations

Several limitations exist within the current system design that should be acknowledged:

**Technical Constraints:**
- Offline functionality is limited to read-only access; users cannot modify data without internet connection
- Package creation or structural changes require developer intervention (version 1 limitation)
- The system is initially designed for 10 vehicles and associated instructors, which may limit scalability
- No native mobile applications are planned; all access is through web browsers
- DMV synchronization frequency depends on their API availability and rate limits

**Operational Constraints:**
- Phone-based scheduling remains a manual process requiring secretary intervention
- Pickup and drop-off locations must be the same for each lesson (no one-way trips)
- Package customization beyond enable/disable requires technical support
- The system cannot automatically assign instructors based on student preferences or performance

**Future Considerations:**
- Multi-language support is not included in version 1
- Geographic expansion to multiple states requires additional DMV integrations
- Advanced analytics and predictive modeling for student success are deferred
- Integration with third-party calendar applications is not supported initially
- Real-time GPS tracking of training vehicles is outside current scope

## Schedule

*Note: As mentioned in the rubric, I would include a Gantt chart screenshot here showing the project timeline from the interview transcript. The chart would display:
- Requirements gathering (14 days)
- Use case and activity diagrams (8 days)
- UI/UX research (9 days, parallel)
- Class diagram development (9 days)
- Customer review meetings (2 days)
- Interface development (12 days)
- Database implementation (9 days)
- Business logic layer (22 days)
- System delivery and sign-off (4 days)*

[Gantt Chart Screenshot Placeholder]

---

*Prepared by Justin Guida for CS 255 Systems Analysis and Design*