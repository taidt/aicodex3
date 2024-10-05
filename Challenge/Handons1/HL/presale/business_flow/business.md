1. User Registration & Login Workflow

sequenceDiagram
  participant User
  participant MobileApp
  participant APIGateway
  participant UserManagementService
  participant OTPService
  participant Admin
  participant PostgreSQLDatabase

  User ->> MobileApp: Start Registration
  MobileApp ->> APIGateway: Send Registration Details
  APIGateway ->> UserManagementService: Validate & Store User Data
  UserManagementService ->> PostgreSQLDatabase: Store Registration Data
  UserManagementService ->> OTPService: Send OTP via SMS

  User ->> MobileApp: Enter OTP
  MobileApp ->> APIGateway: Verify OTP
  APIGateway ->> UserManagementService: OTP Verification
  UserManagementService ->> PostgreSQLDatabase: Update User Status (Pending Approval)

  Admin ->> WebApp: Review User Registration
  WebApp ->> APIGateway: Send Approval Request
  APIGateway ->> UserManagementService: Approve User
  UserManagementService ->> PostgreSQLDatabase: Update User Status (Approved)

  User ->> MobileApp: Login
  MobileApp ->> APIGateway: Send Login Request
  APIGateway ->> UserManagementService: Authenticate User
2. KYC Management Workflow

sequenceDiagram
  participant RM as RelationshipManager
  participant Investor
  participant MobileApp
  participant APIGateway
  participant KYCService
  participant DocumentStorage
  participant KYCVerificationAPI

  RM ->> MobileApp: Initiate KYC
  MobileApp ->> APIGateway: Send KYC Request
  APIGateway ->> KYCService: Process KYC Request
  KYCService ->> DocumentStorage: Store KYC Documents

  alt Face-to-Face KYC
    RM ->> MobileApp: Capture Documents & Signatures
    MobileApp ->> KYCService: Send Documents
  else Remote KYC
    KYCService ->> OTPService: Send KYC Link to Investor
    Investor ->> WebApp: Open KYC Link, Provide Digital Signature
    WebApp ->> KYCService: Submit Signed Document
  end

  KYCService ->> KYCVerificationAPI: Verify Investor Data
  KYCVerificationAPI ->> KYCService: Verification Result
  KYCService ->> PostgreSQLDatabase: Update KYC Status
3. Transaction Management Workflow

sequenceDiagram
  participant RM as RelationshipManager
  participant MobileApp
  participant APIGateway
  participant TransactionService
  participant PostgreSQLDatabase
  participant OTPService
  participant AppMoneyAPI

  RM ->> MobileApp: Initiate Transaction
  MobileApp ->> APIGateway: Send Transaction Request
  APIGateway ->> TransactionService: Validate Transaction Details
  TransactionService ->> PostgreSQLDatabase: Store Transaction Request

  alt OTP Authorization
    TransactionService ->> OTPService: Send OTP
    RM ->> MobileApp: Enter OTP
    MobileApp ->> APIGateway: Verify OTP
    APIGateway ->> TransactionService: OTP Verification
  end

  TransactionService ->> AppMoneyAPI: Execute Transaction
  TransactionService ->> PostgreSQLDatabase: Update Transaction Status
  TransactionService ->> NotificationService: Send Confirmation Notification
4. Portfolio Management and Reporting Flow

sequenceDiagram
  participant RM as RelationshipManager
  participant MobileApp
  participant APIGateway
  participant PortfolioService
  participant PostgreSQLDatabase
  participant NotificationService
  participant ReportingService
  participant PowerBI

  RM ->> MobileApp: Request Portfolio Overview
  MobileApp ->> APIGateway: Fetch Portfolio Data
  APIGateway ->> PortfolioService: Retrieve Portfolio Details
  PortfolioService ->> PostgreSQLDatabase: Query Portfolio and NAV Data
  PortfolioService ->> MobileApp: Return Portfolio Details

  PortfolioService ->> NotificationService: Trigger Alerts for Changes
  NotificationService ->> MobileApp: Push Notification

  RM ->> MobileApp: Generate Report
  MobileApp ->> APIGateway: Request Report Data
  APIGateway ->> ReportingService: Generate Report
  ReportingService ->> PostgreSQLDatabase: Query Relevant Data
  ReportingService ->> PowerBI: Visualize Report
  ReportingService ->> MobileApp: Send Report URL or Download Link