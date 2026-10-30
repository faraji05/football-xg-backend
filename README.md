# Football xG Backend

Django REST Framework starter for the football expected-goals capstone. MongoDB is configured through Django MongoDB Backend. The project intentionally does not define user, match, shot, or CSV schemas yet; those contracts should be agreed on by the team before implementation.

## Requirements

- Python 3.12 or newer
- MongoDB running locally, or a MongoDB connection URI from the team

## Local setup (Windows PowerShell)

```powershell
py -3.12 -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
pip install -r requirements.txt
Copy-Item .env.example .env
python manage.py migrate
python manage.py runserver
```

Set `MONGODB_URI` and `MONGODB_DATABASE` in `.env` to the values shared by the database owner. The defaults target a local MongoDB server at `mongodb://127.0.0.1:27017` and use the `football_xg` database.

## API

- `GET /api/health/` returns `{"status": "ok"}`.
- Django admin is available at `/admin/` after creating a superuser with `python manage.py createsuperuser`.

The health endpoint confirms the API process is responding; it does not check MongoDB connectivity. DRF endpoints require authentication by default, except for the public health endpoint.

## Tests

```powershell
python manage.py test api
```

## GitHub Actions CI/CD

The workflow at `.github/workflows/csc325_ci_cd.yml` runs on pushes to `main`
and pull requests targeting `main`. It has four ordered jobs: Setup,
Dependencies, Test, and Deploy. The Deploy job depends on Test, so GitHub
Actions skips it if the test job fails. Deploy currently prints a placeholder
message; it does not publish a live application.

The workflow uses Python 3.12. To record a feature change with protected
`main`, push the feature branch while its pull request to `main` is open. The
pull-request run can be shown in the video; merging the pull request then
triggers a second run from the push to `main`. In the public GitHub repository,
create a ruleset under **Settings > Rules > Rulesets** that targets `main` and
requires a pull request before merging. The repository itself and its Actions
run pages must be public for the grader.

## Team contracts to confirm

- MongoDB collections and relationships for users, matches, and shots
- Shot CSV columns and coordinate convention before analytics preprocessing is wired in
- Authentication and role-assignment approach before adding user-facing endpoints
