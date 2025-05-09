curl -X POST "http://localhost:8088/api/v1/security/login" -H "Content-Type: application/json" -d "{\"username\":\"admin\",\"password\":\"admin\",\"provider\":\"db\",\"refresh\":true}"
