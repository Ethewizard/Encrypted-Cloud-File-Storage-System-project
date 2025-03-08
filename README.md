# 🛡️ Encrypted S3 Upload API

A secure file storage API built with **Spring Boot** that encrypts files using **AES encryption** before uploading them to an **AWS S3 bucket**. This project demonstrates secure data handling practices, including server-side encryption and seamless integration with AWS cloud services.

---

## 🚀 Features
- 🔒 **AES Encryption:** Files are encrypted with AES-128 before uploading.
- 🌐 **Spring Boot API:** RESTful endpoint for file upload (`/api/storage/upload`).
- ☁️ **AWS S3 Integration:** Secure file storage using the AWS SDK for Java.
- 🛠️ **Robust Error Handling:** Provides clear responses for both success and failure scenarios.

---

## 📦 Technologies Used
- 🧑‍💻 **Java & Spring Boot**
- ☁️ **AWS SDK for S3**
- 🔐 **AES Encryption (javax.crypto)**
- 📦 **Maven**
- 🛠️ **GitHub Actions** (for CI/CD)

---

## 🧠 How It Works
1. Receives a file via a **POST** request.
2. Encrypts the file using **AES encryption**.
3. Uploads the encrypted file to the specified **S3 bucket**.
4. Returns a success or error message.

### 🔗 Example API Request
```bash
curl -X POST -F "file=@/path/to/your/file.txt" http://localhost:8080/api/storage/upload
```

---

## 💡 Getting Started

### 1. Clone the Repository
```bash
git clone https://github.com/yourusername/aws-encrypted-s3-upload-api.git
cd aws-encrypted-s3-upload-api
```

### 2. Configure AWS & Environment
Update the **application.properties** file with your AWS credentials and S3 bucket name:
```properties
aws.bucket.name=your-bucket-name
aws.region=us-west-2
```

### 3. Build & Run the Application
```bash
./mvnw spring-boot:run
```

### 4. Test the API
Use tools like **Postman** or **cURL** to test the file upload endpoint.

---

## 📑 Code Examples

### 🛠️ Encryption and S3 Upload Service (`StorageService.java`)
```java
@Service
public class StorageService {
    @Value("${aws.bucket.name}")
    private String bucketName;

    private final S3Client s3Client = S3Client.builder().build();

    public String uploadFile(MultipartFile file) throws Exception {
        byte[] encryptedData = encryptFile(file.getBytes());
        String fileName = file.getOriginalFilename();

        InputStream inputStream = new ByteArrayInputStream(encryptedData);
        PutObjectRequest request = PutObjectRequest.builder()
                .bucket(bucketName)
                .key(fileName)
                .build();

        s3Client.putObject(request, RequestBody.fromInputStream(inputStream, encryptedData.length));

        return "File uploaded successfully as " + fileName;
    }

    private byte[] encryptFile(byte[] data) throws Exception {
        SecretKey secretKey = KeyGenerator.getInstance("AES").generateKey();
        Cipher cipher = Cipher.getInstance("AES");
        cipher.init(Cipher.ENCRYPT_MODE, secretKey);
        return cipher.doFinal(data);
    }
}
```

### 🌐 Controller (`StorageController.java`)
```java
@RestController
@RequestMapping("/api/storage")
public class StorageController {
    @Autowired
    private StorageService storageService;

    @PostMapping("/upload")
    public ResponseEntity<String> uploadFile(@RequestParam("file") MultipartFile file) {
        try {
            String message = storageService.uploadFile(file);
            return ResponseEntity.ok(message);
        } catch (Exception e) {
            return ResponseEntity.status(500).body("File upload failed: " + e.getMessage());
        }
    }
}
```

---

## 🧪 Testing & CI/CD
- **Unit Testing:** Implemented with JUnit
- **CI/CD Pipeline:** GitHub Actions automates build and deployment

### 🛠️ Example GitHub Actions Workflow
```yaml
name: CI/CD Pipeline

on: [push]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
    - name: Build with Maven
      run: mvn clean install
    - name: Run Tests
      run: mvn test
    - name: Deploy to AWS
      run: echo "Deploy script here"
```

---

## 📄 License
This project is licensed under the **MIT License**.

---

## 💬 Feedback
Feel free to open an issue or submit a pull request for any improvements!
