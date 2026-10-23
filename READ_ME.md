# Studio Ghibli: REST API vs GraphQL

## 1. Project Overview

*Topic:* REST API vs GraphQL

*Objective:* To expose the same small dataset of Studio Ghibli films through a REST API and a read-only GraphQL API, then compare their requests, responses, flexibility, and data-fetching behaviour.

The project uses Python, FastAPI, Strawberry GraphQL, and the public Studio Ghibli API.

## 2. Team Members and Roles

| Member | Name | Responsibility |
|---|---|---|
| Member 1 | ALERIC PINTO | REST API endpoints and error handling |
| Member 2 | DHRUMIL SANGHVI | GraphQL schema, queries, and resolvers |
| Member 3 | TANUSHKA SHAH | Public API integration, data handling, and caching |
| Member 4 | SONAKSHI PRASAD | Testing, comparison, screenshots, and documentation |

All team members should understand the complete project and contribute meaningful Git commits.

## 3. Technologies Used

- Python
- FastAPI
- Strawberry GraphQL
- Requests
- Uvicorn
- Studio Ghibli public API
- Git and GitHub

## 4. Architecture

text
Studio Ghibli Public API
          |
          v
  Python Data Layer
  Fetch + In-Memory Cache
          |
          v
   Shared Film Dataset
       /         \
      v           v
 REST API     GraphQL API
 FastAPI      Strawberry
      \           /
       v         v
    Client Requests


Both interfaces use the same dataset. The data layer caches the fetched results to reduce repeated calls to the public API.

## 5. Project Structure

text
IMAP_Project/
├── app/
│   ├── __init__.py
│   ├── data.py
│   ├── main.py
│   └── graphql_schema.py
├── requirements.txt
└── README.md


## 6. Setup Instructions

### Prerequisites

- Python installed
- VS Code or another Python editor
- Internet access for the initial dataset fetch

### Install dependencies

Activate your virtual environment and run:

bash
pip install -r requirements.txt


### Start the server

Run this command from the project root:

bash
python -m uvicorn app.main:app --reload


### Available interfaces

- REST API documentation: http://127.0.0.1:8000/docs
- All films: http://127.0.0.1:8000/films
- One film: http://127.0.0.1:8000/films/1
- Film director: http://127.0.0.1:8000/films/1/director
- GraphQL interface: http://127.0.0.1:8000/graphql

## 7. REST API Examples

### Get all films

http
GET /films


Returns the available film records.

### Get one film

http
GET /films/1


Returns the film with local ID 1.

### Select particular fields

http
GET /films?fields=title,director


Returns only the title and director fields.

### Error handling

Requesting a film ID that does not exist returns HTTP status code 404.

## 8. GraphQL Examples

### Fetch selected fields from all films

graphql
query {
  films {
    id
    title
    director
  }
}


### Fetch one film

graphql
query {
  film(id: 1) {
    title
    director
    releaseDate
  }
}


### Select only two fields

graphql
query {
  film(id: 10) {
    title
    rtScore
  }
}


GraphQL uses camelCase field names by default in Strawberry. For example, release_date in Python becomes releaseDate in GraphQL.

## 9. Research Questions

### Q1. What are over-fetching and under-fetching?

*Over-fetching* occurs when an API returns more data than the client needs. For example, an application may need only a film's title but receive its title, director, producer, release year, running time, and score.

*Under-fetching* occurs when a client needs additional requests to collect all the information required for a screen. For example, a client may retrieve a film and then make another request for its director details.

GraphQL allows clients to select fields and request related data in one query when the schema supports it. REST can also reduce unnecessary data through endpoint design and field-selection parameters.

### Q2. What are schema, query, and resolver in GraphQL?

- *Schema:* Defines the types and fields that the API makes available.
- *Query:* Specifies the data the client wants to retrieve.
- *Resolver:* A function that obtains the data for a requested field.

In our project, the Film type defines film fields, the Query class defines the available read operations, and its methods resolve requests using the shared film dataset.

### Q3. When would you still choose REST?

REST is a suitable choice when an application has straightforward resource-based operations, predefined response structures, and conventional HTTP caching requirements.

It is also widely supported and can be simpler to implement and maintain when clients need predictable resources rather than flexible field selection.

### Q4. How do REST and GraphQL differ in requests, responses, and flexibility?

REST generally uses resource-specific endpoints and HTTP methods. Its responses are determined by the endpoint's design, although parameters can support field selection.

GraphQL commonly exposes a single endpoint and allows clients to request specific fields through queries. This offers greater flexibility in shaping responses.

Both approaches can return JSON, and neither is automatically faster in every situation. Performance depends on implementation, query complexity, caching, and the amount of data transferred.

## 10. Testing Plan

The following tests should be performed before submission:

1. Retrieve all films through REST.
2. Retrieve one film using its local ID.
3. Request selected fields through the REST fields parameter.
4. Verify that an invalid ID returns HTTP 404.
5. Query all films through GraphQL.
6. Query one film and select only particular fields.
7. Confirm that both APIs return consistent data.
8. Restart the application and verify that it can fetch the data again.
9. Test behaviour when the public API is unavailable.

## 11. Screenshots

Add screenshots of:

1. REST response for all films.
2. REST response for one film.
3. REST response with selected fields.
4. GraphQL query and its response.
5. GraphQL response showing selected fields.
6. Successful test of an invalid film ID.
7. The application running in the terminal.

Insert screenshots here before submission.

## 12. Limitations

- The dataset is limited to the first ten films returned by the public API.
- Local IDs from 1 to 10 are assigned for demonstration; they are not the original API IDs.
- The in-memory cache lasts only while the server process is running.
- The application requires internet access when the cache is empty.
- The application is read-only and does not implement create, update, or delete operations.

## 13. Conclusion

This project demonstrates how REST and GraphQL can expose the same film dataset through different API approaches. REST provides straightforward resource-based endpoints, while GraphQL gives clients more control over the fields returned. Testing both approaches helps illustrate their flexibility, response structures, and practical trade-offs.

## 14. GitHub Repository

*Repository URL:* [Add your group repository URL]

Each member should make meaningful commits and be prepared to explain their contributions and the overall implementation.