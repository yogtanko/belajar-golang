# Belajar Go

## Materi: Basic Routing Gin

### Endpoint API
| Method | Endpoint | Fungsi |
|--------|----------|--------|
| GET | `/users` | Ambil semua user |
| GET | `/users/:id` | Ambil user by ID |
| GET | `/users/search?name=` | Cari user by name |
| POST | `/users` | Buat user baru |
| PUT | `/users/:id` | Update user |
| DELETE | `/users/:id` | Hapus user |

### Kode Penting

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
    id := c.Param("id")           // parameter URL
    name := c.Query("name")       // query parameter
    c.ShouldBindJSON(&user)       // bind JSON request
    c.JSON(http.StatusOK, resp)   // response JSON
}
```

**Concurrency Safety**
```go
mu.RLock()  // untuk GET
mu.Lock()   // untuk POST/PUT/DELETE
defer mu.Unlock()
```

**HTTP Status Code yang Digunakan**
- `http.StatusOK` (200)
- `http.StatusCreated` (201)
- `http.StatusBadRequest` (400)
- `http.StatusNotFound` (404)

### Cara Jalankan
```bash
go get github.com/gin-gonic/gin
go run main.go
```
