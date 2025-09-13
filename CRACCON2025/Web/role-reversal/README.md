# Role Reversal
# Author: [Krishna](https://github.com/krishnast545)

# Solution

Role manipulation
- Use password-reset 
- Intercept and add {role:admin} with the mail to mainpilate the user role
 - Fuzz/guess /api/admin/{tickets} and use the reset token with role:admin  to fetch all tickets
