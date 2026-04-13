---
name: api-design
description: Use when creating, modifying, or reviewing REST API endpoints, routes, or schemas
---

# REST API Design Conventions

Follow these conventions for all API work in this project.

## URL Patterns
- Use plural nouns: `/users`, `/orders`, `/products`
- Use kebab-case for multi-word resources: `/order-items`
- Nest for relationships: `/users/{id}/orders`
- Never use verbs in URLs: `/users` not `/getUsers`
- Version in the URL: `/api/v1/users`

## HTTP Methods
- GET: read (never changes data)
- POST: create new resource
- PUT: replace entire resource
- PATCH: update specific fields
- DELETE: remove resource

## Response Format
- Always return JSON
- Wrap responses: `{ "data": ..., "meta": { "total": N } }`
- Errors: `{ "error": { "code": "NOT_FOUND", "message": "..." } }`
- Use HTTP status codes correctly (201 for created, 404 for not found)

## Naming
- camelCase for JSON fields: `firstName`, `createdAt`
- snake_case for query params: `?sort_by=name&page_size=20`
- Consistent date format: ISO 8601 (`2026-04-13T10:30:00Z`)
