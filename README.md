# Business Import API


| Version      | Date | Description |
| --- | --- | --- |
| 0.1 | 2022/01/27 | Inital creation


## Introduction
This API can be used by registered business partner to create business profile on a [gewerbe.nebenan.de/](https://gewerbe.nebenan.de) platform.

Please see attached postman collection for examples.

### Audience
This document designed for technical staff. This document includes different technical terms, and requires an understanding of these by the reading person.

### Authorization
Every call should have `x-auth-token` header. Calls without that header will be rejected.

Token structure: `partner code:some_secret`

Example:

```
acme_corp:d4322b2879dae36af385e20908c2b50c
```

Example of authorization header:

```
x-auth-token: acme_corp:d4322b2879dae36af385e20908c2b50c
```

**IMPORTANT!**
**Please keep token in secure place and do not share it with third-parties!**


## API usage
Base URLs are:

- **LIVE**: `https://api-nebenan.de/`
- **STAGE**: `https://lisa-api.nebenan.de/` 

**Headers**

```
content-type: application-json
```


### Import a business profile

```
PUT /api/business/v1/imports/:partner_external_id.json
{
    "business_profile": {
        "title": "Example GbR",
        "description": "Example trading since 1984",
        "internal_email": "email@example.dev",
        "website": "https://example.dev",
        "contact_phone": "030-12345678",
        "contact_role": "Inhaber",
        "contact_name": "Some Person",
        "zip_code": "10997",
        "city": "Berlin",
        "street": "Köpenicker Straße",
        "house_number": "154",
        "imprint_freetext": "legal!",
        "imprint_url": "https://nebenan.de/imprint",
        "opening_hours": {
            "monday": [
                [
                    "09:00",
                    "12:00"
                ],
                [
                    "13:00",
                    "18:30"
                ]
            ],
            "tuesday": [],
            "wednesday": [
                [
                    "09:00",
                    "12:00"
                ],
                [
                    "13:00",
                    "18:30"
                ]
            ],
            "thursday": [
                [
                    "09:00",
                    "12:00"
                ],
                [
                    "13:00",
                    "18:30"
                ]
            ],
            "friday": [],
            "saturday": [
                [
                    "09:00",
                    "12:00"
                ],
                [
                    "13:00",
                    "18:30"
                ]
            ],
            "sunday": []
        }
    }
}

```


Query parameters:

| Parameter | Type | Mandatory | Description | Example |
| --- | --- | --- | --- | --- |
| `partner_external_id` | String | YES | External identificator of a profile in partner's backoffice system | `foo1234`

Body parameters:

| Parameter | Type | Mandatory |Description | Example |
| --- | --- | --- | --- | --- |
| `title` | String | + |Business title | `Example GbR`
| `description` | String | + | Description of a business profile | `Example trading since 1984`
| `internal_email` | String | + | ??? | `email@example.dev`
| `website` | String | - | Business website | `https://example.dev` |
| `contact_phone` | String | - | Contact phone |  `030-12345678` |
| `contact_role` | String | - |Contact person's role in a business |  `Inhaber` |
| `contact_name` | String | - |Contact person name | `John Doe` |
| `zip_code` | String | + |zip code | `10997` |
| `city` | String | + |City name | `Berlin` |
| `street` | String | + |Street | `Köpenicker Straße` |
| `house_number` | String | + |House number| `154` |
| `imprint_freetext` | String | - |Legal information in freetxt format |  `legal!` |
| `imprint_url` | String | - |Link to the legal information | `https://nebenan.de/imprint` |
| `opening_hours` | Object |  - |JSON object containing opening hours for every weekday | see example below |

```
"opening_hours": {
  "monday": [
      [
          "09:00",
          "12:00"
      ],
      [
          "13:00",
          "18:30"
      ]
  ],
  "tuesday": [],
  "wednesday": [
      [
          "09:00",
          "12:00"
      ],
      [
          "13:00",
          "18:30"
      ]
  ],
  "thursday": [
      [
          "09:00",
          "12:00"
      ],
      [
          "13:00",
          "18:30"
      ]
  ],
  "friday": [],
  "saturday": [
      [
          "09:00",
          "12:00"
      ],
      [
          "13:00",
          "18:30"
      ]
  ],
  "sunday": []
}
```

**Response**

```
HTTP-RESPONSE-CODE: 200
BODY: null
```

### Update business profile
```
PUT /api/business/v1/imports/:partner_external_id.json
{
  "business_profile": {
    "title": "Updated Title",
    "internal_email": "test@example.com",
    "zip_code": "10997",
    "city": "Berlin",
    "street": "Köpenicker Straße",
    "house_number": "154"
  }
}
```

Query string and body parameters are the same as is previous 

**Response**

Success:

```
HTTP-RESPONSE-CODE: 200
BODY:
null
```

### Delete imported business profile

```
DELETE /api/business/v1/imports/:partner_external_id.json
```

**Response**

Success:

```
HTTP-RESPONSE-CODE: 204
BODY:
""
```

Error:

```
HTTP-RESPONSE-CODE: 404
BODY:
{
  "error": "Business profile not previously imported"
}
```
