
# GET
- **GET method**: retrieves information or data from a specified resource

```python 
@app.get("/get_specialities")
async def get_specialities():  
    try:
        specialities_document_id ="asone:code:specialite"
        # Check if the specialities document exists in the database
        if specialities_document_id not in db_asone:
            raise HTTPException(status_code=404, detail="specialities document not found")
        # Retrieve the existing specialities document by ID 
        specialities_document = db_asone[specialities_document_id]["specialites"]
        #print(specialities_document)
        return JSONResponse(content=specialities_document,  headers={"Access-Control-Allow-Origin": "*"})
    except ResourceNotFound:
        raise HTTPException(status_code=404, detail="specialities document not found")
        
```

#### 1. Decorator `@app.get("/get_specialities")`
This decorator is from FastAPI, It tells FastAPI to handle `GET` requests sent to the `/get_specialities` endpoint using the `get_specialities` function.


#### 2. Function Definition

`async def get_specialities():`

The function `get_specialities` is defined as asynchronous (using `async`). This allows the function to handle input/output operations (like database access) without blocking the execution of other operations.

#### 3. Try Block

``` python
try:     specialities_document_id = "asone:code:specialite"
```

The `try` block begins by defining a variable `specialities_document_id`, which holds the ID of the document we are looking for in the database.

#### 4. Checking Document Existence

``` python 
if specialities_document_id not in db_asone:     
	raise HTTPException(status_code=404, detail="specialities document not found")
```

Here, it checks if `specialities_document_id` exists in the `db_asone` database. If the document does not exist, it raises an `HTTPException` with a 404 status code and a detail message saying "specialities document not found". This makes sure that if the document is not found, the client receives a clear and accurate error message.

#### 5. Retrieving Document

```python 
specialities_document = db_asone[specialities_document_id]["specialites"]`
```

If the document exists, it retrieves the document from the database using its ID and specifically accesses the `"specialites"` field within that document.

#### 6. Returning the Response

```python
return JSONResponse(content=specialities_document, headers={"Access-Control-Allow-Origin": "*"})`
```

The retrieved document is returned as a JSON response. The `JSONResponse` is from FastAPI's `starlette.responses` module, which converts the `specialities_document` dictionary to a JSON-formatted response. Additionally, it sets the `Access-Control-Allow-Origin` header to `"*"`, allowing cross-origin requests from any domain. ([[CORS]])

#### 7. Except Block


```python 
except ResourceNotFound:     
	raise HTTPException(status_code=404, detail="specialities document not found")
```

The `except` block catches any `ResourceNotFound` exceptions, which would occur if the resource is not found in the database. When such an exception is caught, it raises another `HTTPException` with a 404 status code and a detail message indicating that the "specialities document not found". 


# POST
 - submits data to be processed to a specified resource 

# PUT
- updates a specified resource with new data
# DELETE 
- deletes a specified resource