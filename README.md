# openAPI
## README

---

### OpenAPI Course

This repository contains the OpenAPI specification (version 3.0.2) for an educational course on OpenAPI. The specification defines the API structure, endpoints, and data models for managing customers, beer information, and beer orders.

#### Information

- **Version:** 1.0
- **Title:** OpenAPI course
- **Description:** Specification for OpenAPI Course
- **Terms of Service:** [http://example.com/terms/](http://example.com/terms/)
- **Contact:**
  - **Name:** Joanna B.
  - **Email:** email@gmail.com
  - **URL:** [http://example.com/](http://example.com/)
- **License:**
  - **Name:** Apache 2.0
  - **URL:** [https://www.apache.org/licenses/LICENSE-2.0.html](https://www.apache.org/licenses/LICENSE-2.0.html)

#### Servers

- Development Server: [http://dev.example.com/](http://dev.example.com/)
- Test Server: [http://test.example.com/](http://test.example.com/)
- Production Server: [http://prod.example.com/](http://prod.example.com/)

#### Paths

##### `/v1/customers`

- **GET:** List Customers
  - Summary: List Customers
  - Description: Get a list of customers in the system
  - Tags: Customers
  - Operation ID: listCustomersV1
  - Parameters:
    - Page Number Parameters
    - Page Size Parameters
  - Responses:
    - 200: List of Customers
    - 404: No Customers found
  - Security: None

- **POST:** New Customer
  - Summary: New Customer
  - Description: Create a new customer
  - Tags: Customers
  - Request Body:
    - Required: true
    - Content: JSON (See schema: `#/components/schemas/customers`)
  - Responses:
    - 201: Customer Created
    - 400: Bad Request
    - 409: Conflict
  - Callbacks:
    - Order Status Change (Webhook for order status change notifications)
      - POST:
        - Request Body:
          - Content: JSON (See schema: `#/components/schemas/orderStatusChange`)
        - Responses:
          - 200: Ok

##### `/v1/customers/{customersId}`

- **GET:** Get Customer By ID
  - Summary: Get Customer By ID
  - Description: Get a single Customer by its ID value
  - Tags: Customers
  - Operation ID: getCustomerByIdV1
  - Parameters:
    - Customers ID Path Parameter
  - Responses:
    - 200: Found Customer
    - 404: Not found
  - Security: None

- **PUT:** Update Customer
  - Summary: Update Customer by Id
  - Description: Update Customer by Id
  - Tags: Customers
  - Parameters:
    - Customers ID Path Parameter
  - Request Body:
    - Required: true
    - Content: JSON (See schema: `#/components/schemas/customers`)
  - Responses:
    - 204: Customers Updated
    - 400: Bad Request
    - 409: Conflict

- **DELETE:** Delete Customer By ID
  - Summary: Delete Customer By ID
  - Description: Delete a single Customer by its ID value
  - Tags: Customers
  - Operation ID: deleteCustomerByIdV1
  - Parameters:
    - Customers ID Path Parameter
  - Responses:
    - 200: Customer deleted
    - 404: Not found

##### `/v1/beer`

- **GET:** Get Beer
  - Summary: Get Beer
  - Description: Get a Beer
  - Tags: Beer
  - Operation ID: getBeer
  - Parameters:
    - Page Number Parameters
    - Page Size Parameters
  - Responses:
    - 200: List of beer
    - 404: Not found

- **POST:** Create Beer
  - Summary: Create Beer
  - Description: Create a Beer
  - Tags: Beer
  - Operation ID: createBeerV1
  - Parameters:
    - Page Number Parameters
    - Page Size Parameters
  - Request Body:
    - Required: true
    - Content: JSON (See schema: `#/components/schemas/beer`)
  - Responses:
    - 201: Created Beer
    - 400: Bad Request
    - 409: Conflict

##### `/v1/beer/{beerId}`

- **GET:** Get Beer By ID
  - Summary: Get Beer By ID
  - Description: Get a beer by its ID value
  - Tags: Beer
  - Operation ID: getBeerByIdV1
  - Parameters:
    - Beer ID Path Parameter
  - Responses:
    - 200: Found Beer
    - 404: Not found

- **PUT:** Update Beer
  - Summary: Update Beer by Id
  - Description: Update Beer by Id
  - Tags: Beer
  - Parameters:
    - Beer ID Path Parameter
  - Request Body:
    - Required: true
    - Content: JSON (See schema: `#/components/schemas/beer`)
  - Responses:
    - 204: Beer Updated
    - 400: Bad Request
    - 409: Conflict

- **DELETE:** Delete Beer By ID
  - Summary: Delete Beer By ID
  - Description: Delete a single Beer by its ID value
  - Tags: Beer
  - Operation ID: deleteBeerByIdV1
  - Parameters:
    - Beer ID Path Parameter
  - Responses:
    - 200: Beer deleted
    - 404: Not found

#### Security

- Basic Authentication
- JWT Authentication

#### Components

##### Security Schemes

- BasicAuth
  - Type: HTTP
  - Scheme: Basic
- JwtAuthToken
  - Type: HTTP
  - Scheme: Bearer
  - Bearer Format: JWT

##### Schemas

- `address`
  - Type: Object
  - Properties:
    - line1: String
    - city: String
    - stateCode: Enum (AL, BA, CD, XS, XD)
    - zipcode: String
- `customers`
  - Type: Object
  - Properties:
    - id: UUID
    - firstName: String
    - lastName: String
    - address: Ref to `#/components/schemas/address`
- `customers_list`
  - Type: Array
  - Items: Ref to `#/components/schemas/customers`
- `brewery`
  - Type: Object
  - Properties:
    - name: String
    - location: String
- `beer`
  - Type: Object
  - Properties:
    - UUID: UUID
    - name: String
    - style: Enum (ALE, PALE_ALE, IPA, WHEAT, LAGER)
    - price: Float
    - brewery: Ref to `#/components/schemas/brewery`
- `List_of_beer`
  - Type: Array
  - Items: Ref to `#/components/schemas/beer`
- `BeerP

agedList`
  - Type: Object
  - AllOf:
    - Ref to `#/components/schemas/PagedResponse`
  - Properties:
    - content: Ref to `#/components/schemas/List_of_beer`
- `BeerOrder`
  - Type: Object
  - Properties:
    - id: UUID
    - customerId: UUID
    - customerRef: String
    - beerOrderLines: Array of Ref to `#/components/schemas/BeerOrderLine`
    - orderStatusCallbackUrl: URI
- `BeerOrderLine`
  - Type: Object
  - Properties:
    - id: UUID
    - beerId: UUID
    - upc: String
    - orderQuantity: Integer
    - quantityAllocated: Integer
- `PagedResponse`
  - Type: Object
  - Properties:
    - pageable: Ref to `#/components/schemas/PagedResponse_pageable`
    - totalPages: Integer
    - last: Boolean
    - totalElements: Integer
    - size: Integer
    - number: Integer
    - numberOfElements: Integer
    - sort: Ref to `#/components/schemas/PagedResponse_pageable_sort`
    - first: Boolean
- `PagedResponse_pageable_sort`
  - Type: Object
  - Properties:
    - sorted: Boolean
    - unsorted: Boolean
- `PagedResponse_pageable`
  - Type: Object
  - Properties:
    - sort: Ref to `#/components/schemas/PagedResponse_pageable_sort`
    - offset: Integer
    - pageNumber: Integer
    - pageSize: Integer
    - paged: Boolean
    - unpaged: Boolean

##### Parameters

- `pageNumberParameters`
  - Name: pageNumber
  - In: Query
  - Description: Page Number
  - Schema: Integer (Format: int32, Default: 1)
- `pageSizeParameters`
  - Name: pageSize
  - In: Query
  - Description: Page Size
  - Required: false
  - Schema: Integer (Format: int32, Default: 25)
- `CustomersIdPathParm`
  - Name: customersId
  - In: Path
  - Description: Customers Id
  - Required: true
  - Schema: String (Format: uuid)
- `BeerIdPathParm`
  - Name: beerId
  - In: Path
  - Description: Beer Id
  - Required: true
  - Schema: String (Format: uuid)

---

This OpenAPI specification defines a comprehensive API structure for managing customers, beer information, and beer orders. It includes various endpoints, security schemes, and data models to facilitate the development of associated applications. Feel free to use and adapt this specification as needed for your projects.
