
>[!info] check this [repo](https://github.com/OsswaCh/FastAPI_endPoint_Doctor_app.git) for the details about testing this code 
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

here we will be taking the example of creating a new doctor document: suppose when he first signs in

```python
	class Doctor (BaseModel):
    name: str
    speciality: str
    phone: str
    email: str
    address: str
    city: str
    description: str


@app.post("/add_doctor")
async def add_doctor(doctor: Doctor):
    try:
        doc_id = "asone:code:doctors"

        # creation of a new document
        doc = {
            "id": doc_id,
            "name": doctor.name,
            "speciality": doctor.speciality,
            "phone": doctor.phone,
            "email": doctor.email,
            "address": doctor.address,
            "city": doctor.city,
            "description": doctor.description
        }

        db_asone.save(doc)

        return JSONResponse(content={"message": "doctor added successfully"}, headers={"Access-Control-Allow-Origin": "*"})
    except Exception as e:
        raise HTTPException(status_code=500, detail=f"An error occurred: {str(e)}")
```

here the class doctor can be changed to a simple dictionary, the example just follows this structure 

#### 1. create the object
here doc components are passed to the function and assigned later
#### 2. Save
`db_asone.save(doc)` saves the variable in the db

# PUT
- updates a specified resource with new data

```python
@app.put("/add_speciality")
async def add_speciality(speciality: str):
    try:
        specialties_document_id = "asone:code:specialite"
        # Check if the specialities document exists in the database
        if specialties_document_id not in db_asone:
            raise HTTPException(status_code=404, detail="specialities document not found")

        # Retrieve the existing document 
        doc = db_asone[specialties_document_id]

        # get the current specialities and make sure it is a list 
        specialties = doc.get("specialites", [])
        if isinstance(specialties, str):
            specialties = [specialties] 

        # add the new speciality to the list
        if speciality not in specialties:
            specialties.append(speciality)

        # update the document with the new specialities
        doc["specialites"] = specialties
        db_asone.save(doc)

        return JSONResponse(content={"message": "speciality added successfully"}, headers={"Access-Control-Allow-Origin": "*"})
    except ResourceNotFound:
        raise HTTPException(status_code=404, detail="specialities document not found")
```

database format: any addition later will be added to the specialties array 
```NoSQL
{
  "_id": "asone:code:specialite",
  "_rev": "6-4c45f887c1a0c345ae21472d59d44279",
  "specialites": [
    "osswaaaaaaaaaaa",
    "new specialty",
    "haja"
  ]
}
```

- **Retrieve the Field with a Default Value**
    
    `specialities = doc.get("specialites", [])`
    
    This line attempts to retrieve the value associated with the key `"specialites"` from the `doc` dictionary. The `get` method is used here, which returns the value of the key if it exists; otherwise, it returns the default value provided as the second argument. In this case, the default value is an empty list (`[]`). This ensures that if the `"specialites"` key is not present in the document, `specialities` will be initialized as an empty list.
    
- **Check if the Field is a String**
    
    `if isinstance(specialities, str):`
    
    The `isinstance` function checks if `specialities` is an instance of the `str` class. This condition is true if the value retrieved from the `"specialites"` field is a string.
    
- **Convert String to Array**
    
    `specialties = [specialties]`
    
    If `specialities` is indeed a string, this line converts it into a list containing that string. This transformation ensures that `specialities` is always an array, even if the input was a single string.
-  Save
	`db_asone.save(doc)` saves the variable in the db

# DELETE 
- deletes a specified resource
this example deletes a whole document

```python 
@app.delete("/delete_doctor")
async def delete_doctor(doctor_id: str):
    try: 
        if doctor_id not in db_asone:
            raise HTTPException(status_code=404, detail="doctor not found, cannot be removed")
        doc= db_asone[doctor_id]
        db_asone.delete(doc)
        return JSONResponse(content={"message": "doctor deleted successfully"}, headers={"Access-Control-Allow-Origin": "*"})
    except ResourceNotFound:
        raise HTTPException(status_code=404, detail="doctor not found, cannot be removed")


```

this however is a form of update but can be considered a delete 
```python 

# -------- delete a speciality ---------- #
@app.delete("/delete_speciality")
async def add_speciality(speciality: str):
    try:
        specialties_document_id = "asone:code:specialite"
        # Check if the specialities document exists in the database
        if specialties_document_id not in db_asone:
            raise HTTPException(status_code=404, detail="specialities document not found")

        # Retrieve the existing document 
        doc = db_asone[specialties_document_id]

        # get the current specialities and make sure it is a list 
        specialties = doc.get("specialites", [])
        if isinstance(specialties, str):
            specialties = [specialties] 

        # delete the speciality from the list
        if speciality in specialties:
            specialties.remove(speciality)

        # update the document with the new specialities
        doc["specialites"] = specialties
        db_asone.save(doc)

        return JSONResponse(content={"message": "speciality added successfully"}, headers={"Access-Control-Allow-Origin": "*"})
    except ResourceNotFound:
        raise HTTPException(status_code=404, detail="specialities document not found")
    
```