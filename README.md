curl -X POST http://localhost:8088/api/v1/security/login \
  -H "Content-Type: application/json" \
  -d '{
    "username": "admin",
    "password": "yourpassword",
    "provider": "db",
    "refresh": true
}'
