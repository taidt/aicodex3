*1. System Architecture Overview*

graph TD
  User -->|Uses| MobileApp
  Admin -->|Uses| WebApp
  Investor -->|Uses| MobileApp

  MobileApp -->|Sends Requests| APIGateway
  WebApp -->|Sends Requests| APIGateway

  APIGateway --> UserManagementService
  APIGateway --> KYCService
  APIGateway --> TransactionService
  APIGateway --> PortfolioService

  UserManagementService -->|CRUD Operations| PostgreSQLDatabase
  KYCService -->|Upload/Download KYC Files| DocumentStorage
  TransactionService -->|CRUD Operations| PostgreSQLDatabase
  PortfolioService -->|CRUD Operations| PostgreSQLDatabase

  KYCService --> KYCVerificationAPI
  UserManagementService --> OTPService
  TransactionService --> AppMoneyAPI

  NotificationService -->|Push Notifications| MobileApp
  NotificationService -->|Email, SMS Alerts| ExternalServices
