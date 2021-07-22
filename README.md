<p align="center">
  <a href="https://github.com/nadtakanf/gopher-holes-unlimited">
    <img src="images/aws-community-builder.png" alt="Logo" width="80" height="80">
  </a>

  <h3 align="center">Gopher Holes Unlimited</h3>
</p>

<!-- TABLE OF CONTENTS -->
<details open="open">
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#about-the-project">About The Project</a>
      <ul>
        <li><a href="#architecture-Gopher-backend-api-diagram">Architecture Gopher Backend API diagramh</a></li>
        <li><a href="#built-with">Built With</a></li>
        <li><a href="#business-requirements">Business Requirements</a></li>
        <li><a href="#database-schema">Database Schema</a></li>
        <li><a href="#open-api-spec">Open API Spec</a></li>
      </ul>
    </li>
  </ol>
</details>

<!-- ABOUT THE PROJECT -->
## About The Project
Welcome to Gopher Holes Unlimited! A new website that tracks two things: gophers and gopher holes.
We pride ourselves in our ability to provide current, up-to-date statistics on all gophers and their
associated holes in the United States.

#### Architecture Gopher Backend API diagram
<img src="/images/gopher-holes-unlimited-arch-diagram.png"/>

### Business Requirements
* A user can add/update/delete gophers from the system
* A user can add/update/delete gopher holes from the system
* A user can associate an existing gopher hole to a gopher
* A user can change the status of a gopher
    * Statuses: At Large, Captive, Terminated
    * When the gopher status changes, an email is automatically sent to all “watchers”
* A user can search for gophers by type, name, status, or location
* The system must publish a webhook event when a gopher hole is created or deleted
* If a user delete a gopher, all associated gopher holes must also be deleted automatically

### Built With
This project is built with technologiest as below.
* [Serverless Framework](https://www.serverless.com/)
* [NodeJS](https://nodejs.org/en/)
* [DynamoDB](https://aws.amazon.com/dynamodb/)
* [DynamoDB Single Table Design](https://www.youtube.com/watch?v=Q6-qWdsa8a4)
* [Cognito Presignup Triggers](https://docs.aws.amazon.com/cognito/latest/developerguide/user-pool-lambda-pre-sign-up.html)
* [AWS SDK V3](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html)
* [AWS Event Bridge](https://www.youtube.com/watch?v=28B4L1fnnGM)
* [AWS Lambda](https://aws.amazon.com/lambda/)
* [Cognito JWT token](https://docs.aws.amazon.com/cognito/latest/developerguide/amazon-cognito-user-pools-using-tokens-with-identity-providers.html)
* [AWS SNS](https://aws.amazon.com/sns/)
* [AWS SQS](https://aws.amazon.com/sqs/)
* [Github Action](https://github.com/features/actions)
* [Lambda log](https://lambdalog.js.org/)

### Database Schema
Attempt | #1 | #2 | #3 | #4 | #5 | #6 | #7 | #8 | #9 | #10 | #11
--- | --- | --- | --- |--- |--- |--- |--- |--- |--- |--- |---
Seconds | 301 | 283 | 290 | 286 | 289 | 285 | 287 | 287 | 272 | 276 | 269

PK  | #SK | #id | #email | #name | #gopherStatus | #user | #holes | #createdAt 
--- | --- | --- | --- | --- | --- | --- | --- | --- |
USER#tigerwoods  | USER#tigerwoods   |       | tigerwoods@gmail.com | Tiger Woods | At Large      | tigerwoods   |        | 1626275538869
USER#tigerwoods  | Hole#ulid         | ulid  |                      |             |               |              |   6    | 1626275538869
USER#wacher1     | Wacher#tiberwoods |       | wacher1 @gmail.com   | wacher 1    |               | wacher1      |        | 1626275538869 

### Open API Spec

Gopher
----

Create gopher
```paths:
  /gopher/create:
    post:
      summary: Add a new gopher
      requestBody:
        content:
          application/json:
            schema:      # Request body contents
              type: object
              properties:
                pk:
                    type: string
                sk:
                    type: string
                id:
                    type: string
                email:
                    type: string
                name:
                    type: string
                gopherStatus:
                    type: string
                user:
                    type: string
                createAt:
                    type: string
              example:   # Sample object
                pk: USER#tigerwoods  
                sk: USER#tigerwoods
                email: tigerwoods@gmail.com
                name: Tiger Woods
                gopherStatus: At Large
                user: tigerwoods
                createAt: 1626275538869
      responses:
        '200':
          description: OK
```

Update gopher
```paths:
  /gopher/update/{id}:
    post:
      summary: Update a gopher
      requestBody:
        content:
          application/json:
            schema:      # Request body contents
              type: object
              properties:
                pk:
                    type: string
                sk:
                    type: string
                id:
                    type: string
                email:
                    type: string
                name:
                    type: string
                gopherStatus:
                    type: string
                user:
                    type: string
                createAt:
                    type: string
              example:   # Sample object
                pk: USER#tigerwoods  
                sk: USER#tigerwoods
                email: tigerwoods@gmail.com
                name: Tiger Woods
                gopherStatus: At Large
                user: tigerwoods
                createAt: 1626275538869
      responses:
        '200':
          description: OK
```

Delete gopher
```paths:
  /gopher/delete/{id}:
    post:
      summary: Delete a gopher
      requestBody:
        content:
          application/json:
            schema:      # Request body contents
              type: object
              properties:
                id:
                    type: string
              example:   # Sample object
                id: ulid
      responses:
        '200':
          description: OK
```

Search gophers by type, name, status, or location
```paths:
  /gopher/search?gopherType=Professional:
    post:
      summary: Search gophers by type, name, status, or location
      requestBody:
        content:
          application/json:
            schema:      # Request body contents
              type: object
              properties:
                gopherType:
                    type: string
                name:
                    type: string
                gopherStatus:
                    type: string
                location:
                    type: string
              example:   # Sample object
                gopherType: Professional
                name: Tiger Woods
                gopherStatus: Captive
                location: USA
      responses:
        '200':
          description: OK
```

----

Gopher hole 
----

Create a gopher hole
```paths:
  /gopher-hole/create:
    post:
      summary: Add a new gopher hole
      requestBody:
        content:
          application/json:
            schema:      # Request body contents
              type: object
              properties:
                pk:
                    type: string
                sk:
                    type: string
                id:
                    type: string
                hole:
                    type: number
                createAt:
                    type: string
              example:   # Sample object
                pk: USER#tigerwoods  
                sk: Hole#ulid
                id: ulid
                hole: 6
                createAt: 1626275538869
      responses:
        '200':
          description: OK
```

Update gopher
```paths:
  /gopher/update/{id}:
    post:
      summary: Update a gopher
      requestBody:
        content:
          application/json:
            schema:      # Request body contents
              type: object
              properties:
                pk:
                    type: string
                sk:
                    type: string
                id:
                    type: string
                hole:
                    type: number
                createAt:
                    type: string
              example:   # Sample object
                pk: USER#tigerwoods  
                sk: Hole#ulid
                id: ulid
                hole: 6
                createAt: 1626275538869
      responses:
        '200':
          description: OK
```

Delete gopher hole
```paths:
  /gopher-hole/delete/{id}:
    post:
      summary: Delete a gopher
      requestBody:
        content:
          application/json:
            schema:      # Request body contents
              type: object
              properties:
                id:
                    type: string
              example:   # Sample object
                id: ulid
      responses:
        '200':
          description: OK
```