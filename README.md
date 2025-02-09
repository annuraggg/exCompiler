# exCompiler

Welcome to exCompiler, a cloud-based code execution platform. This service is currently in its beta phase and not suitable for production use.

## Overview

exCompiler provides a seamless solution for integrating multi-language code execution capabilities into your web applications.

## Setup

In order to setup this project locally on your machine, you can either run the docker image or run the nodejs app directly using node. Although running directly using node is not recommended since we're dealing with installing so many languages like java, c, c++, etc. You'd need to install SDKs and Compilers for each of the language supported by this API for testing purposes.

### Install project (using Docker)

```bash
docker build --no-cache -t ex-compiler .

docker run -p 3000:3000 ex-compiler
```

## API Reference

### Code Execution Endpoint

**Endpoint:** `POST /`

This primary endpoint facilitates code execution and returns the computed results.

#### Required Parameters

| Field | Purpose | Details |
|-------|---------|---------|
| code | Source code | The program to be executed |
| language | Programming language identifier | Specified using the language codes below |
| input | Program input data | Optional; required only for interactive programs |

#### Supported Programming Languages

The platform automatically selects the latest stable compiler version for each supported language.

| Programming Language | Identifier Code |
|---------------------|-----------------|
| Python | py |
| Java | java |
| C++ | cpp |
| C | c |
| NodeJS | js |
| GoLang | go |
| C# | cs |


### Implementation Example

Here's a NodeJS implementation showcasing how to interact with the API:

```javascript
var axios = require('axios');
var qs = require('qs');

const executionData = qs.stringify({
    'code': 'userVal = int(input("Value: ")) + 10\nprint(userVal)',
    'language': 'py',
    'input': '15'
});

const requestConfig = {
    method: 'post',
    url: 'https://api.excompiler.dev',
    headers: {
        'Content-Type': 'application/x-www-form-urlencoded'
    },
    data: executionData
};

axios(requestConfig)
    .then(response => console.log(JSON.stringify(response.data)))
    .catch(error => console.log(error));
```

### Response Format

The API returns a JSON object with the following structure:

```json
{
    "timeStamp": 1672439982964,
    "status": 200,
    "output": "Value: 25\n",
    "error": "",
    "language": "py",
    "info": "Python 3.6.9\n"
}
```

### Language Information Endpoint

**Endpoint:** `GET /list`

Retrieve information about supported languages and their compiler versions.

Sample Response:
```json
{
    "timeStamp": 1672440064864,
    "status": 200,
    "supportedLanguages": [
        {
            "language": "py",
            "info": "Python 3.6.9\n"
        },
        {
            "language": "java",
            "info": "openjdk 11.0.17 2022-10-18\n"
        }
        // Additional language entries...
    ]
}
```

## Important Notes

- The API supports cross-origin requests, enabling seamless frontend integration
---
Created with ❤️ by Anurag Sawant
