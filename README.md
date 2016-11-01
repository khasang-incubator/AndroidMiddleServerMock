# AndroidMiddleServerMock

##API

###Base URL
http://109.234.34.97:4000/

###Access
All requests should be contains header "token". Only request "/api/goods/" without "token" header return goods list and token new User with role=guest. This token need save on client. This insert for requests for identification user on service.
Token structure: {version}&{Id user}&{random field}. You can get IdUser from token.
User with role guest can getting list goods only. 
For uploaded goods, images (for goods) you user should up role to "user". It's easy: need upload for link http://109.234.34.97:4000/api/trust/ JSON model User contans fields "email" and "token". This request should be signed header "token" of user. Service send for email message, contains link completion registration.
#####Sample JSON model User:
{        
        "email": "sample@yandex.ru", 
        "token": "v1&5aa3a914-####-####-####-29785ab24ddf&3c7416f4-1be3-46c0-8892-14213a282bebe9da6748-10a8-428b-8ae2-a7876ac4cd7e",
        "role": "guest",
        "active": true
}

###GET goods
http://109.234.34.97:4000/api/goods/
Note: thit pagination response. Params: page, pageSize.
Sample request: http://109.234.34.97:4000/api/goods/?page=2&pageSize=20
If params is not defined => default value http://109.234.34.97:4000/api/goods/?page=1&pageSize=30
You can GET only one good. http://109.234.34.97:4000/api/goods/{id}

###Add good
POST http://109.234.34.97:4000/api/goods/ JSON model Good. Fields "title", "description", "cost", "ownerId" must be valid! Header "token" is contains true token user with role=user (after registration).
#####Sample JSON good model
{
    "title":"Samsung Galaxy S7 Edge 32Gb", 
    "ownerId":"5aa3a914-####-####-####-29785ab24ddf", 
    "description":"Элегантный дизайн и ...",
    "cost":61790.0,
    "photos":[{"link":"55e872c0-2485-422e-b704-7d191b5a96a1.jpg"},{"link":"99600091-4714-402d-8d5e-6a786cc745ba.jpg"},{"link":"cac69fc4-43b5-43e8-b9f0-f1136c03c54a.jpg"},{"link":"4ae0fa4c-1edd-4227-b0b5-5d58f349227f.jpg"},{"link":"5f2f1dc2-b7d8-42a3-bf0d-fee8e38f38a8.jpg"},{"link":"fd96844f-4772-4383-8068-dd812e65f898.jpg"}]
}

###Upload images.
JSON model of upload good desirable to provide array of images. Service supports upload, resizing an hosting images and thumbnail. You need POST http://109.234.34.97:4000/api/upload/ with field "uploaded_file" contains file image (ext JPG only). Request should be signed header "token". Response will contains BASE_NAME this image on hosting. For get resize image ask: http://109.234.34.97:4000/s-BASE_NAME. For get thumbnail ask: ask: http://109.234.34.97:4000/m-BASE_NAME. Note: uploaded images that are not referenced by any item will be removed (delay a few hours).

###Update and delete goods. 
User good owner can update (PUT) and remove (DELETE) his goods just send PUT http://109.234.34.97:4000/api/goods/{id} or DELETE http://109.234.34.97:4000/api/goods/{id}. PUT request shoulds contains valid JSON model Good. Request should be signed header "token" owner this good! 
