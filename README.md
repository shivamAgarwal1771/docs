,
'"password":' is not recognized as an internal or external command,
operable program or batch file.

C:\Users\Shivam220802\superset>    "provider": "db",
'"provider":' is not recognized as an internal or external command,
operable program or batch file.

C:\Users\Shivam220802\superset>    "refresh": true
'"refresh":' is not recognized as an internal or external command,
operable program or batch file.

C:\Users\Shivam220802\superset>}'
'}'' is not recognized as an internal or external command,
operable program or batch file.

C:\Users\Shivam220802\superset>curl -X POST http://localhost:8088/api/v1/security/login \  -H "Content-Type: application/json" \  -d '{    "username": "admin",    "password": "admin",    "provider": "db",    "refresh": true}'
{
  "message": "400 Bad Request: Failed to decode JSON object: Expecting value: line 1 column 1 (char 0)"
}
curl: (3) URL rejected: Bad hostname
curl: (3) URL rejected: Bad hostname
curl: (3) URL rejected: Port number was not a decimal number between 0 and 65535
curl: (3) URL rejected: Bad hostname
curl: (3) URL rejected: Port number was not a decimal number between 0 and 65535
curl: (3) URL rejected: Bad hostname
curl: (3) URL rejected: Port number was not a decimal number between 0 and 65535
curl: (3) URL rejected: Bad hostname
curl: (3) URL rejected: Port number was not a decimal number between 0 and 65535
curl: (3) unmatched close brace/bracket in URL position 5:
true}'
    ^
