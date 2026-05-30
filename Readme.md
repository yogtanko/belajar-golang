# Learn Go

## Module 1: Basic Routing Gin

### Endpoint API

| Method | Endpoint              | Function          |
| ------ | --------------------- | ----------------- |
| GET    | `/users`              | Get all users     |
| GET    | `/users/:id`          | Get user by ID    |
| GET    | `/users/search?name=` | Find user by name |
| POST   | `/users`              | Create new user   |
| PUT    | `/users/:id`          | Update user       |
| DELETE | `/users/:id`          | Delete user       |

### Important Code

**Router Setup**

```go
router := gin.Default()
router.GET("/users", getAllUsers)
router.POST("/users", createUser)
router.GET("/users/:id", getUserByID)
router.PUT("/users/:id", updateUser)
router.DELETE("/users/:id", deleteUser)
router.GET("/users/search", searchUsers)
router.Run()
```

**Handler Pattern**

```go
func handler(c *gin.Context) {
    id := c.Param("id")           // URL parameter
    name := c.Query("name")       // query parameter
    c.ShouldBindJSON(&user)       // bind JSON request
    c.JSON(http.StatusOK, resp)   // JSON response
}
```

**Concurrency Safety**

```go
mu.RLock()  // for GET
mu.Lock()   // for POST/PUT/DELETE
defer mu.Unlock()
```

**HTTP Status Codes Used**

- `http.StatusOK` (200)
- `http.StatusCreated` (201)
- `http.StatusBadRequest` (400)
- `http.StatusNotFound` (404)

---

## Module 2: Middleware Gin

### Endpoint API

| Method | Endpoint        | Function                                      |
| ------ | --------------- | --------------------------------------------- |
| GET    | `/ping`         | Health check endpoint                         |
| GET    | `/articles`     | Get all articles                              |
| GET    | `/articles/:id` | Get article by ID                             |
| POST   | `/articles`     | Create new article (API Key required)         |
| PUT    | `/articles/:id` | Update article (API Key required)             |
| DELETE | `/articles/:id` | Delete article (API Key required)             |
| GET    | `/admin/stats`  | Get API statistics (API Key & Admin required) |

### Important Code

**Middleware Registration Order**

```go
router := gin.New()                  // Without default middleware
router.Use(ErrorHandlerMiddleware()) // Handle panic & recovery
router.Use(RequestIDMiddleware())    // Add UUID request header
router.Use(LoggingMiddleware())      // Log request details & duration
router.Use(CORSMiddleware())         // Setup CORS header & OPTIONS preflight
router.Use(RateLimitMiddleware())    // IP rate limiter
router.Use(ContentTypeMiddleware())  // Enforce application/json for POST/PUT
```

**Rate Limiter per IP**

```go
// Limit max 100 requests per IP per minute
clients.LoadOrStore(ip, &IPClient{
    limiter: rate.NewLimiter(rate.Every(1*time.Minute/100), 100),
})
```

**Role-Based Authorization**

```go
// Validate API Key to user role mapping
userRole := map[string]string{
    "admin-key-123": "admin",
    "user-key-456":  "user",
}
```

**HTTP Status Codes Used**

- `http.StatusOK` (200)
- `http.StatusCreated` (201)
- `http.StatusNoContent` (204)
- `http.StatusBadRequest` (400)
- `http.StatusUnauthorized` (401)
- `http.StatusForbidden` (403)
- `http.StatusNotFound` (404)
- `http.StatusUnsupportedMediaType` (415)
- `http.StatusTooManyRequests` (429)
- `http.StatusInternalServerError` (500)

---

## Module 3: Input Validation Gin

### Endpoint API

| Method | Endpoint            | Function                                   |
| ------ | ------------------- | ------------------------------------------ |
| POST   | `/products`         | Create new product with custom validation  |
| POST   | `/products/bulk`    | Validate & create multiple products (bulk) |
| POST   | `/categories`       | Create new category                        |
| POST   | `/validate/sku`     | Check SKU format validity & uniqueness     |
| POST   | `/validate/product` | Validate product data without saving       |
| GET    | `/validation/rules` | Get list of active validation rules        |

### Important Code

**Struct Binding Validation Tags**

```go
type Product struct {
    SKU         string    `json:"sku" binding:"required"`
    Name        string    `json:"name" binding:"required,min=3,max=100"`
    Price       float64   `json:"price" binding:"required,min=0.01"`
    Category    Category  `json:"category" binding:"required"`
    Inventory   Inventory `json:"inventory" binding:"required"`
}
```

**Custom Format Validators**

```go
// Validate SKU format (format: ABC-123-XYZ)
const skuFormat = `^[A-Z]{3}-\d{3}-[A-Z]{3}$`

// Validate Warehouse Code format (format: WH###)
const warehouseFormat = `^WH\d{3}$`
```

**Sanitization & Calculation**

```go
// Trim whitespace & dynamically calculate available stock
product.Name = strings.TrimSpace(product.Name)
product.Inventory.Available = product.Inventory.Quantity - product.Inventory.Reserved
```

**HTTP Status Codes Used**

- `http.StatusOK` (200)
- `http.StatusCreated` (201)
- `http.StatusBadRequest` (400)
- `http.StatusUnprocessableEntity` (422)

---

### How to Run

**Running the Main Module (Module 1 & Module 2)**
Adjust the active function call in [main.go](file:///d:/Koding/golang/playground/main.go), then run:

```bash
go get github.com/gin-gonic/gin
go run main.go
```

**Running the Validation Module (Module 3)**
This module can be executed directly by targeting its file:

```bash
go run belajar-gin/validation/validation.go
```
