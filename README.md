## HTTP-JS Zip Code Search Demo

![alt text](image.png) 

This application is a cutting-edge demonstration of WebAssembly (WASM) embedded ZIP code lookup functionality, designed to operate on an EDGE server architecture distributed across eight nations. Built to ensure both speed and security, this application leverages the robustness of WASM to facilitate efficient and fast ZIP code queries directly from the client's browser, reducing server load and enhancing user experience. It is uniquely configured for a NO-OPS architecture, eliminating the need for traditional operational overhead. This makes it an ideal solution for developers and businesses looking for a scalable, maintenance-free geographic data retrieval system that emphasizes privacy and performance.

![image](https://github.com/w3point0/zip-code-embedded/assets/993459/0e2b1485-4bbf-4c3e-8a03-989c11b62814)  
[Demo: Click Here](https://zip-code-embedded-rlrioea8.fermyon.app/zip?code=89144)  
Change the Zip Code to see the result  

### Build

```console
npm install
spin build
```

### Run

```console
spin up
```

Or, use `spin watch` to run the app and rebuild on any changes to `package.json` or the files in `src`.

Use e.g. `curl -v http://127.0.0.1:3000/zip` to test the endpoint.

### Speed 𝑂(1)

In Big O notation, which measures how well an algorithm scales, accessing an item in a hash table by its key usually takes constant time, denoted as 𝑂(1). This means the time it takes to find an item doesn't increase with the size of the data. This efficiency comes from how hash tables are built to quickly retrieve data, assuming that the data is evenly distributed and doesn't bunch up too much in one place.

### Embeded Table

42,735 Rows of Zip Code latitudes and longitudes 

```
"99927": { "lat": 56.33, "lng": -133.61 },
"99928": { "lat": 55.45, "lng": -131.79 },
"99929": { "lat": 56.41, "lng": -131.61 },
"99950": { "lat": 55.34, "lng": -131.64 }
```

### The Code - Not Much. Less is more! 
```
// Type declaration for handling HTTP requests
export const handleRequest: HandleRequest = async function (request: HttpRequest): Promise<HttpResponse> {
    // Initialize default location and response status
    let location: { lat: number; lng: number } | undefined;
    let status = 200;
    const requiredZipLength = 5;

    // Extract ZIP code from the URL query parameter 'code'
    const url = new URL(request.uri);
    const params = new URLSearchParams(url.search);
    let zipCode = params.get('code') || '0';

    // Ensures the ZIP code is of the required length, padding with zeros if necessary
    function formatZipCode(input: string): string {
        if (input.length < requiredZipLength) {
            return input.padStart(requiredZipLength, '0');
        }
        return input;
    }
    zipCode = formatZipCode(zipCode);

    // Handle GET requests by looking up location data based on the ZIP code
    if (request.method === "GET") {
        location = zipData[zipCode as keyof typeof zipData];
        if (!location) {
            // Set location to a default value and update status to indicate a bad request
            location = { lat: 0, lng: 0 };
            status = 400;
        }
    }
    
    // Return the HTTP response with the location data and status code
    return {
        status: status,
        body: JSON.stringify(location)
    };
}

```

[Fermyon.com](https://www.fermyon.com/)  
![image](https://github.com/w3point0/zip-code-embedded/assets/993459/7cb7b6ed-4652-45e4-a89d-41c5ca67ecef)


