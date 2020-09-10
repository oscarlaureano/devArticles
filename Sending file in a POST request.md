# Sending file in a POST request 

I had some troubles trying to send a file via **POST** using Typescript 2.7 and Axios, so I wanted to write my solution here, hopefully it will be helpful to others.

The method I was trying to use is to simply make an object and send it as the body of the Post request:

```typescript
// Headers object
const headers = { 'userID': "1234" };
// Body Object
const body = { 
    'processID': "4321",
    'file': fs.createReadStream( "files/data.zip")
    };
// URL
const url = 'localhost:8080/api/Extension/deploy';
// POST request
let results = await axios.post(url, body, { headers });
// Showing results
console.log(results);
```

But the file was not reaching its final destination, so after looking for a while I found that it worked using **form-data** to construct the body of the request:

```typescript
// Importing library
const FormData = require('form-data');
// Headers object
const headers = { 'userID': "1234" };
// Creating a new form data
var bodyFormData = new FormData();
// Appending info to body form data
await bodyFormData.append('processID', "1234");
await bodyFormData.append('file', fs.createReadStream("files/data.zip"), "data.zip"); 
//URL
const url = 'localhost:8080/api/Extension/deploy';
// POST request
let results = await axios.post(url, body, { headers });
// Showing results
console.log(results);
```