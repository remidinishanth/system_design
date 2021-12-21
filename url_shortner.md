REF: https://github.com/donnemartin/system-design-primer/blob/master/solutions/system_design/pastebin/README.md and https://www.codekarle.com/system-design/TinyUrl-system-design.html

### Functional Requirements
* Get Short URL
* Given a short url, redirect to long URL
* Expiry & Analytics (advanced)

### Non-Functional Requirements
* Very Low latency
* Very high availability


### Length of Short URL
* Based on the customers/users per month
* Which all characters will you be using for this, based on the unique number of URLs you are going to generate

### How to convert Long URL to short URL
* Can try using MD5 for creating a shortURL
* Create a token service, which can return a range of numbers(say 1000-2000), and it stores in DB that these numbers are assigned, whenever an instance of app needs tokens, it can call this token service and get the range of numbers it can use, convert this number to base62 and return the tinyURL
