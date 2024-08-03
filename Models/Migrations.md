# Migrations

Make migrations
```powershell
python manage.py makemigrations app
```

SQL output of this migration
```powershell
python manage.py sqlmigrate app 0001
```

Apply migration
```
python manage.py migrate
```
