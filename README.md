# FastAPI Book Management API

## Overview

This project is a FastAPI-based Book API that allows users to retrieve book details by ID. It includes CI/CD automation using GitHub Actions and is deployed with Nginx as a reverse proxy.

## Features

- 📚 Book management (CRUD operations)
- ✅ Input validation using Pydantic models
- 🔍 Enum-based genre classification
- 🧪 Complete test coverage
- 📝 API documentation (auto-generated by FastAPI)
- 🔒 CORS middleware enabled

## Project Structure

```
hngx-stage2-fastapi-book-project/
├── api/
│   ├── db/
│   │   ├── __init__.py
│   │   └── schemas.py      # Data models and in-memory database
│   ├── routes/
│   │   ├── __init__.py
│   │   └── books.py        # Book route handlers
│   └── router.py           # API router configuration
├── core/
│   ├── __init__.py
│   └── config.py           # Application settings
├── tests/
│   ├── __init__.py
│   └── test_books.py       # API endpoint tests
├── main.py                 # Application entry point
├── nginx.conf              # Nginx configuration for reverse proxy   
├── requirements.txt        # Project dependencies
└── README.md
```

## Technologies Used

- Python 3.12
- FastAPI
- Pydantic
- pytest
- uvicorn
- ngork

## Setup & Installation

1. Clone the repository:

```bash
git clone [[https://github.com/YOUR_USERNAME/fastapi-book-project.git](https://github.com/dany-gaga/hngx-stage2-fastapi-book-project/tree/main)](https://github.com/dany-gaga/hngx-stage2-fastapi-book-project)
cd hngx-stage2-fastapi-book-project
```

2. Create a virtual environment:

```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

3. Install dependencies:

```bash
pip install -r requirements.txt
```

## Running the Application locally

1. Start the server:

```bash
uvicorn main:app
```
Now, visit http://127.0.0.1:8000/docs to see the API documentation.

2. Access the API documentation:

- Swagger UI: http://localhost:8000/docs
- ReDoc: http://localhost:8000/redoc

## API Endpoints
1. Get book by id

###Endpoint: `GET /api/v1/books/{book_id}`
###Response: `200 OK`: Returns book details in JSON format.
              `404 Not Found`: If the book does not exist.

###Example
```json
{
    "id": 1,
    "title": "The Great Gatsby",
    "author": "F. Scott Fitzgerald"
}
```

⚙️ CI/CD Pipeline

✅ CI Pipeline (Testing on PRs)

- Runs `pytest` on pull requests to `main`.
- Fails if tests fail.
- Succeeds if all tests pass.

🚀 CD Pipeline (Deployment on Merge to main)

Pulls the latest changes on EC2.

Restarts Uvicorn service.

Ensures the API is running via Nginx.


🌍 Deployment

1️⃣ Configure Nginx as a Reverse Proxy

Update the Nginx configuration file:

```json
server {  
    listen 80;  
    server_name 54.210.215.74;  # Using the public IP 

    location / {  
        proxy_pass http://127.0.0.1:8000;  # Ensure Uvicorn is running here  
        proxy_http_version 1.1;  
        proxy_set_header Upgrade $http_upgrade;  
        proxy_set_header Connection 'upgrade';  
        proxy_set_header Host $host;  
        proxy_cache_bypass $http_upgrade;  
    }  
} 
```
Restart Nginx:

```bash
sudo systemctl restart nginx
```
Start Uvicorn and ngrok as a Service

```bash
sudo systemctl status uvicorn
```
```bash
sudo systemctl status ngrok
```

### Books

- `GET /api/v1/books/` - Get all books
- `GET /api/v1/books/{book_id}` - Get a specific book
- `POST /api/v1/books/` - Create a new book
- `PUT /api/v1/books/{book_id}` - Update a book
- `DELETE /api/v1/books/{book_id}` - Delete a book

### Health Check

- `GET /healthcheck` - Check API status

## Book Schema

```json
{
  "id": 1,
  "title": "Book Title",
  "author": "Author Name",
  "publication_year": 2024,
  "genre": "Fantasy"
}
```

Available genres:

- Science Fiction
- Fantasy
- Horror
- Mystery
- Romance
- Thriller

## Running Tests

```bash
pytest
```

## Error Handling

The API includes proper error handling for:

- Non-existent books
- Invalid book IDs
- Invalid genre types
- Malformed requests

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit changes (`git commit -m 'Add AmazingFeature'`)
4. Push to branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Support

For support, please open an issue in the GitHub repository.
