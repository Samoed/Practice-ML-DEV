.DEFAULT_GOAL := all
DIRS = src/
poetry = poetry run
isort = isort
black = black
mypy = mypy
flake8  = flake8
pyupgrade = pyupgrade --py311-plus


.PHONY: install-linting
install-linting:
	poetry add ruff -G dev

.PHONY: format
format:
	ruff $(DIRS)
	mypy $(DIRS)

.PHONY: export-dependencies
export-dependencies:
	poetry export -f requirements.txt --output requirements.txt
	poetry export -f requirements.txt --output requirements-dev.txt --with=dev

.PHONY: database
database:
	docker compose up database -d

.PHONY: redis
redis:
	docker compose up redis -d

.PHONY: worker
worker:
	docker compose up worker -d --build

.PHONY: test
test:
	poetry run pytest --cov=app --cov-report=html

.PHONY: migrate
migrate:
	@read -p "Enter migration message: " message; \
	poetry run alembic revision --autogenerate -m "$$message"

.PHONY: downgrade
downgrade:
	alembic downgrade -1

.PHONY: upgrade
upgrade:
	alembic upgrade +1

.PHONY: upgrade-offline
upgrade-offline:
	alembic upgrade head --sql

.PHONY: all
all: format export-dependencies
