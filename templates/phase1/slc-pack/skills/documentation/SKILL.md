---
name: documentation
description: Technical documentation skill. Activate when user says "write docs", "API docs", "README", "user manual", or "update documentation".
version: "1.0"
author: "slc-team"
license: "MIT"
tags: ["documentation", "API", "README", "guide"]
---

# Documentation

Technical documentation writing and maintenance, ensuring accuracy, completeness, and readability.

## Use Cases

- Writing README or getting started guides
- Generating API documentation
- Writing user manuals or operation guides
- Updating existing documentation

## Not Suitable For

- Code comments (should be inline in code)
- Requirements documents (should use requirements skill)
- Architecture documents (should use architecture-design skill)

## Core Capabilities

### Documentation Generation
Generate documentation from code or features.

**Method:**
1. Understand target audience
2. Determine documentation type
3. Extract key information
4. Organize structure

### Documentation Updates
Synchronize existing documentation with code changes.

**Method:**
1. Locate related documentation
2. Compare with code changes
3. Update relevant content
4. Verify documentation consistency

### Format Optimization
Choose appropriate documentation structure and format.

**Method:**
1. Select appropriate documentation type
2. Use clear heading hierarchy
3. Add code examples
4. Include visual elements when necessary

## Usage

### Trigger
```
/documentation --type README
/documentation --type API
/documentation --type guide
/documentation --action update
```

### Input
- Target documentation type
- Related code or feature description
- Target audience

## Output Format Example

```markdown
## API Reference: User Service

### Overview
User service provides user management and authentication.

### Authentication
All APIs require Bearer Token authentication.

### Endpoints

#### GET /users/{id}
Get user information

**Parameters:**
| Name | Type | Location | Description |
|------|------|----------|-------------|
| id | string | path | User ID |

**Response:**
```json
{
  "id": "usr_123",
  "name": "John Doe",
  "email": "john@example.com"
}
```

**Error Codes:**
| Code | Description |
|------|-------------|
| 404 | User not found |
| 401 | Unauthorized |

### Examples

```bash
curl -X GET https://api.example.com/users/usr_123 \
  -H "Authorization: Bearer $TOKEN"
```

## Quick Start

1. Get API Key
2. Make first request
3. Handle response

## Troubleshooting

| Problem | Cause | Solution |
|---------|-------|----------|
| 401 error | Token expired | Refresh token |
| 429 error | Rate limited | Lower request frequency |
```

## Notes

- Documentation should stay in sync with code
- Use clear and concise language
- Provide runnable code examples
- Regularly review documentation accuracy
