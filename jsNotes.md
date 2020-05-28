## Promises 
- introduced to javascript in ES6
- an object that handles asynchronous data 
  - pending: when promise is created or waiting for data
  - fulfilled: when the asynchronous operation was successfully handled 
  - rejected: when the asynchronous operation was unsuccessful 
- once the promise is either successful/unsuccessful, additional methods can be chained to the original promise 

    fetch() function 
- creates a request object that contains relevant info for the API 
- sends request object to API endpoint 
- returns a promise that eventually resolves to a response object with the status of the promise with info from the API 

    ```javascript
    fetch("https://api-to-call.com/endpoint").then(response => {
    if (response.ok) {
    return response.json();
    }
    // throw new Error('Request failed');
    }, networkError => {
    })
    ```
