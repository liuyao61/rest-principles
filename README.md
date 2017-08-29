# Restful API Convention

The API design of superloop portal need to follow the following guidelines:

### HTTP Method

  - GET: getting a resource
  - POST: creating a resource
  - PUT: replacing a resource
  - PATCH: partially replacing a resource
  - DELETE: remove/deactive a resource

Note:
  With some frontend technnologies, you might not be able to use PATCH. You can use PUT instead. In theory, your API need to handle a DTO(json object) with all variables when using PUT otherwise 400(Bad Request) will be thrown while it only need a DTO with partial variables. For example, if the full service DTO is "{ id : 1000, name: 'DF1 between DC1 and DC2', status: 'DRAFT'}". To update the service you need PUT json data like this: "{ id : 1000, name: 'Updated title DF1 between DC1 and DC2', status: 'DRAFT'}" and to put service into provisioning, you need to PATCH "{ id : 1000, status: 'DRAFT'}" 

### URL/Body/Header Design

 - Locators - E.g. resource identifier needs to be in the url path. For example /user/1/ NOTE: We need to have locators for GET/PUT/PATCH and do NOT need identifier in the DTO. For example: PUT /user/19 Request body: "{firstName: 'John', lastName: 'Smith'}". The 'id' field is not needed and won't be checked against the locator in the path. The return needs to be ""{id: 19, firstName: 'John', lastName: 'Smith'}""
 - Filters - E.g. parameters that provides a search for, paging, sorting or narrow down the result sets. For example, /user?company-id=16&page=1
 - State - E.g. web token, session, api key and etc need to be in the relevant request headers. For example, JWT token string needs to be in Authorization header as Authorization: Bearer <token>. Note: we've decided to use local storage with JWT but we may use combined cookie/local storage in future. If for the later approach, we can set the data through cookie as well.
 - Content - E.g. data to be processed and stored need to be in request body.
 - Word Seperator - Use hyphen instead of camelCase or under_score. E.g. /service-locations?company-id=61
 - Singular/Plural - Use plural for a set of resources e.g. /services?tag=BNE and singular for an individual result /service/61. 

More on Singular/Plural  
 - POST /service = create one service
 - POST /services = create a list of services including one service
 - GET /services = get a list of services
 - GET /service/61 = get service with id = 61

### HTTP Status Codes

 - 200 OK - Response to a successful GET, PUT, PATCH or DELETE. Can also be used for a POST that doesn't result in a creation.
 - 201 Created - Response to a POST that results in a creation. Should be combined with aLocation header pointing to the location of the new resource
 - 204 No Content - Response to a successful request that won't be returning a body (like a DELETE request)
 - 304 Not Modified - Used when HTTP caching headers are in play
 - 400 Bad Request - The request is malformed, such as if the body does not parse
 - 401 Unauthorized - When no or invalid authentication details are provided. Also useful to trigger an auth popup if the API is used from a browser
 - 403 Forbidden - When authentication succeeded but authenticated user doesn't have access to the resource
 - 404 Not Found - When a non-existent resource is requested
 - 405 Method Not Allowed - When an HTTP method is being requested that isn't allowed for the authenticated user
 - 410 Gone - Indicates that the resource at this end point is no longer available. Useful as a blanket response for old API versions
 - 415 Unsupported Media Type - If incorrect content type was provided as part of the request
 - 422 Unprocessable Entity - Used for validation errors
 - 429 Too Many Requests - When a request is rejected due to rate limiting
