# Encrypted-Cloud-File-Storage-System-project

🛡️ Encrypted S3 Upload API

A secure file storage API built with Spring Boot that encrypts files using AES encryption before uploading them to an AWS S3 bucket. This project demonstrates secure data handling practices, including server-side encryption and seamless integration with AWS cloud services.


🚀 Features

AES Encryption: Files are encrypted with AES-128 before uploading.

Spring Boot API: RESTful endpoint for file upload (/api/storage/upload).

AWS S3 Integration: Secure file storage using the AWS SDK for Java.

Robust Error Handling: Provides clear responses for both success and failure scenarios.

📂 Technologies Used

Java & Spring Boot

AWS SDK for S3

AES Encryption (javax.crypto)

🧠 How It Works

Receives a file via a POST request.

Encrypts the file using AES encryption.

Uploads the encrypted file to the specified S3 bucket.

Returns a success or error message.

💡 Getting Started

Clone the repository.

Configure AWS credentials and set the BUCKET_NAME in the environment.

Run the Spring Boot application.

Test the API using tools like Postman or cURL.
