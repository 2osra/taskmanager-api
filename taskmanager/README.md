# Task Management System API

Complete Django REST Framework project with JWT + OTP authentication.

## Setup

```bash
pip install -r requirements.txt
python manage.py makemigrations authentication projects tasks
python manage.py migrate
python manage.py createsuperuser
python manage.py runserver
```

## API Endpoints

### Auth
| Method | URL | Description |
|--------|-----|-------------|
| POST | /api/auth/register/ | Register (sends OTP to email) |
| POST | /api/auth/verify-otp/ | Verify OTP → activate account |
| POST | /api/auth/resend-otp/ | Resend OTP |
| POST | /api/auth/login/ | Login → get JWT tokens |
| POST | /api/auth/logout/ | Logout (blacklist refresh token) |
| GET  | /api/auth/profile/ | Get logged-in user profile |

### Projects
| Method | URL | Description |
|--------|-----|-------------|
| GET  | /api/projects/ | List all projects |
| POST | /api/projects/ | Create project (auth required) |
| GET  | /api/projects/{id}/ | Project detail |
| PUT  | /api/projects/{id}/ | Update (owner only) |
| DELETE | /api/projects/{id}/ | Delete (owner only, no incomplete tasks) |

### Tasks
| Method | URL | Description |
|--------|-----|-------------|
| GET  | /api/tasks/ | List tasks (with filters) |
| POST | /api/tasks/ | Create task |
| GET  | /api/tasks/{id}/ | Task detail |
| PUT  | /api/tasks/{id}/ | Update task |
| DELETE | /api/tasks/{id}/ | Delete task (project owner only) |

### Task Filters (GET /api/tasks/)
- `?status=todo` / `in_progress` / `done`
- `?priority=low` / `medium` / `high`
- `?search=keyword`
- `?project_id=1`
- `?ordering=due_date` / `-due_date`
- `?page=1&per_page=10`

## Business Rules
- Only authenticated users can create projects/tasks
- Only project owner can delete a project
- Project cannot be deleted if it has incomplete tasks
- Task cannot be marked Done without an assignee
- Only the assigned user can mark a task as Done
- Due date cannot be set in the past

## OTP Settings (in settings.py)
- OTP expires in 10 minutes
- Uses your Gmail SMTP (jawad291212@gmail.com)
- Make sure App Password is correct

## Run Tests
```bash
python manage.py test tests
```
