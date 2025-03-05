### **Summary: Setting Up Jenkins with HTTPS Using WAR and Tomcat on Windows**  

This guide sets up **Jenkins** (`.war`) on **Tomcat**, enables **HTTPS** using **Apache HTTP Server**, and configures SSL with **OpenSSL**.  

---

### **✅ Prerequisites**  
Download and install the following software:  
| Component         | Version       | Download Link |
|------------------|-------------|---------------|
| **Java (JDK 21)** | 21.0.6 | [IBM Semeru JDK 21](https://github.com/ibmruntimes/semeru21-binaries/releases/download/jdk-21.0.6%2B7_openj9-0.49.0/ibm-semeru-open-jdk_x64_windows_21.0.6_7_openj9-0.49.0.msi) |
| **Tomcat** | 10.1.36 | [Apache Tomcat 10](https://dlcdn.apache.org/tomcat/tomcat-10/v10.1.36/bin/apache-tomcat-10.1.36-windows-x64.zip) |
| **Jenkins WAR** | 2.492.1 | [Jenkins WAR](https://get.jenkins.io/war-stable/2.492.1/jenkins.war) |
| **Apache HTTP Server** | 2.4.63 | [Apache HTTP Server](https://www.apachelounge.com/download/VS17/binaries/httpd-2.4.63-250207-win64-VS17.zip) |
| **OpenSSL** | 3.4.1 | [OpenSSL Light](https://slproweb.com/download/Win64OpenSSL_Light-3_4_1.msi) |

---

### **🛠 Step 1: Set Up Java**
1. **Install JDK 21** and set environment variables:  
   - `JAVA_HOME = C:\Program Files\IBM\SemeruJDK-21`
   - Add `%JAVA_HOME%\bin` to `PATH`.

2. Verify Java installation:  
   ```sh
   java -version
   ```

---

### **🛠 Step 2: Deploy Jenkins WAR in Tomcat**
1. Extract **Tomcat** to `E:\Softwares\tomcat\`.
2. Copy `jenkins.war` to `E:\Softwares\tomcat\webapps\`.
3. Start Tomcat:  
   ```sh
   E:\Softwares\tomcat\bin\startup.bat
   ```
4. Open Jenkins:  
   ```
   http://localhost:8080/jenkins
   ```

---

### **🛠 Step 3: Generate SSL Certificate (Self-Signed)**
1. **Create a Keystore for Tomcat**:  
   ```sh
   keytool -genkeypair -alias tomcat -keyalg RSA -keysize 2048 -validity 365 -keystore E:\Softwares\tomcat\conf\keystore.jks -storepass changeit -dname "CN=jenkins.example.com, OU=IT, O=DS, L=PN, S=MH, C=IN"
   ```
2. **Verify the Keystore**:  
   ```sh
   keytool -list -keystore E:\Softwares\tomcat\conf\keystore.jks -storepass changeit
   ```

---

### **🛠 Step 4: Install Apache HTTP Server & OpenSSL**
1. Install **Apache HTTP Server** and **OpenSSL**.
2. Add OpenSSL to `PATH`:  
   ```
   C:\Program Files\OpenSSL-Win64\bin
   ```

---

### **🛠 Step 5: Generate SSL Certificate for Apache**
1. **Generate Private Key**:  
   ```sh
   openssl genrsa -out E:\Softwares\Apache24\conf\private.key 2048
   ```
2. **Create Certificate Signing Request (CSR)**:  
   ```sh
   openssl req -new -key E:\Softwares\Apache24\conf\private.key -out E:\Softwares\Apache24\conf\certificate.csr
   ```
3. **Generate Self-Signed Certificate**:  
   ```sh
   openssl x509 -req -days 365 -in E:\Softwares\Apache24\conf\certificate.csr -signkey E:\Softwares\Apache24\conf\private.key -out E:\Softwares\Apache24\conf\certificate.crt
   ```

---

### **🛠 Step 6: Configure Apache for Reverse Proxy**
1. Open `E:\Softwares\Apache24\conf\extra\httpd-ssl.conf` and add:
   ```apache
   <VirtualHost _default_:443>
       DocumentRoot "E:\Softwares\Apache24\htdocs"
       ServerName jenkins.example.com:443

       SSLEngine on
       SSLCertificateFile "E:\Softwares\Apache24\conf\certificate.crt"
       SSLCertificateKeyFile "E:\Softwares\Apache24\conf\private.key"

       ProxyPreserveHost On
       ProxyPass "/jenkins" "http://localhost:8080/jenkins"
       ProxyPassReverse "/jenkins" "http://localhost:8080/jenkins"
   </VirtualHost>
   ```
2. Restart Apache:
   ```sh
   httpd -k restart
   ```

---

### **🛠 Step 7: Configure Hosts File**
1. Open `C:\Windows\System32\drivers\etc\hosts` as **Administrator**.
2. Add the following line:
   ```
   127.0.0.1  jenkins.example.com
   ```
3. Save and restart Apache.

---

### **✅ Final Test**
- Open in the browser:
  ```
  https://jenkins.example.com/jenkins
  ```
- If you see a **certificate warning**, click **Advanced → Proceed Anyway**.

---

### **🎯 Summary**
✔ Installed **Java 21**, **Tomcat 10**, **Jenkins WAR**, **Apache HTTP Server**, and **OpenSSL**.  
✔ Configured **SSL for Tomcat** and **Apache HTTP Server**.  
✔ Set up **Reverse Proxy** to serve Jenkins securely over **HTTPS**.  

🚀 **Now Jenkins is accessible via:**  
👉 `https://jenkins.example.com/jenkins` 🎉  
