# Bitespeed Backend Task: Identity Reconciliation

This is a TypeScript-based web service designed to handle identity reconciliation for the Bitespeed platform. The service is responsible for linking and consolidating customer contact information based on email and phone number data.

## Prerequisites

Before running the service, ensure that you have the following installed on your system:

- Node.js (v12 or higher)
- npm (Node Package Manager)

## Installation

1. Clone this repository to your local machine.

2. Navigate to the project directory.

shell
cd bitespeed-backend-task

3.Install the dependencies using npm. 
``npm install``
 Usage
1. Start the web service.
``npm start``
2. The service will be available at http://localhost:3000.
3. Use an HTTP client (e.g., cURL, Postman) to send POST requests to the /identify endpoint with the required JSON payload.
``POST http://localhost:3000/identify
Content-Type: application/json

{
  "email": "mcfly@hillvalley.edu",
  "phoneNumber": "123456"
}``
4. The service will respond with a consolidated contact information JSON payload.
``{
  "contact": {
    "primaryContatctId": 1,
    "emails": ["lorraine@hillvalley.edu","mcfly@hillvalley.edu"],
    "phoneNumbers": ["123456"],
    "secondaryContactIds": [23]
  }
}
``
Database
The service requires a relational database to store and retrieve contact information. You can set up your preferred SQL database and provide the connection details in the .env file. Please refer to the .env.example file for the required environment variables.

Contributing
Contributions to this project are welcome. If you find any issues or want to suggest improvements, please open an issue or submit a pull request.

License
This project is licensed under the MIT License.
