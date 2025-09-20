# Agent OS Context Template: Springfield Academy CMS
## Template Version: 1.0
## Purpose: Reusable context for AI assistants (Claude, Cursor, etc.)

---

# PROJECT CONTEXT

You are helping build a Wagtail CMS website for Springfield Commonwealth Academy. This document contains all technical context needed to assist with development tasks.

## Current Project State
- **Phase**: 0 (Foundation/Bake-off)
- **Template**: TBD (evaluating News Template vs Starter Kit)
- **Timeline**: Week 1 of 8
- **Environment**: GitPod/Ona cloud development

## Technology Stack

### Core Framework
```python
# Versions (exact - do not deviate)
Django==5.0.3
wagtail==7.0.0  # Updated to latest LTS as per global tech stack
psycopg2-binary==2.9.9
```

### Frontend Stack
```javascript
// Package versions
"tailwindcss": "^3.4.0",
"@tailwindcss/forms": "^0.5.7",
"@tailwindcss/typography": "^0.5.10"
```

### Infrastructure
- **Development**: GitPod/Ona (cloud IDE)
- **Staging**: Fly.io (free tier)
- **Production**: Fly.io ($30-50/mo)
- **Database**: PostgreSQL 16
- **Media Storage**: Cloudflare R2
- **CDN**: Cloudflare

## Project Structure

```
springfield/
├── apps/
│   ├── core/              # ALWAYS PUT BASE MODELS HERE
│   ├── programs/          # Program pages and logic
│   ├── faculty/           # Faculty profiles
│   ├── events/            # Event management
│   ├── admissions/        # Forms and CTAs
│   └── news/              # News/blog articles
├── templates/
│   ├── base.html          # MUST INCLUDE ADMISSIONS HEADER
│   └── components/        # Reusable template parts
├── static/
│   ├── css/
│   │   ├── input.css      # Tailwind source
│   │   └── output.css     # Compiled CSS
│   └── js/
└── requirements/
    ├── base.txt           # Core dependencies
    └── production.txt     # Production only
```

## CRITICAL PATTERNS TO FOLLOW

### 1. Wagtail Model Pattern (ALWAYS USE THIS)
```python
from django.db import models  # NOT django.db.models
from wagtail.models import Page  # NOT wagtail.core.models
from wagtail.fields import RichTextField, StreamField
from wagtail.admin.panels import FieldPanel  # NOT wagtail.admin.edit_handlers
from wagtail import blocks

class YourModel(Page):
    # Fields
    field_name = models.CharField(max_length=255)
    
    # StreamFields MUST have use_json_field=True
    content = StreamField([
        ('heading', blocks.CharBlock()),
        ('paragraph', blocks.RichTextBlock()),
    ], use_json_field=True)  # CRITICAL: Don't forget this
    
    # Always include SEO
    meta_description = models.TextField(max_length=160, blank=True)
    
    # Admin panels (new syntax)
    content_panels = Page.content_panels + [
        FieldPanel('field_name'),
        FieldPanel('content'),
    ]
```

### 2. Template Pattern (ADMISSIONS-FIRST)
```django
{# Every template must extend base.html #}
{% extends "base.html" %}
{% load wagtailcore_tags %}

{% block content %}
    {# Admissions CTAs must be prominent #}
    <div class="admissions-ctas">
        <a href="/inquire" class="btn-primary">Inquire</a>
        <a href="/visit" class="btn-secondary">Visit</a>
        <a href="/apply" class="btn-tertiary">Apply</a>
    </div>
    
    {# Page content #}
    {{ page.content|richtext }}
{% endblock %}
```

### 3. Tailwind Classes Pattern
```html
<!-- Color Palette (locked - do not change) -->
<!-- Primary: #003366 (--academy-blue) -->
<!-- Accent: #FFB81C (--academy-gold) -->
<!-- Text: #4A5568 (--academy-gray) -->

<!-- Standard component classes -->
<div class="container mx-auto px-4">  <!-- Standard container -->
<h1 class="text-4xl font-serif text-academy-blue">  <!-- Headings -->
<button class="bg-academy-gold hover:opacity-90 px-6 py-3 rounded">  <!-- CTAs -->
<div class="grid grid-cols-1 md:grid-cols-3 gap-6">  <!-- Grid layouts -->
```

## COMMON TASKS

### Task: Create New Page Model
```python
# STEP 1: Create model file
# apps/[appname]/models.py

# STEP 2: Import requirements (USE EXACT IMPORTS ABOVE)

# STEP 3: Define model class extending Page

# STEP 4: Add to apps/[appname]/admin.py if needed

# STEP 5: Create template
# templates/[appname]/[model_name].html

# STEP 6: Run migrations
python manage.py makemigrations
python manage.py migrate
```

### Task: Add New App
```bash
# STEP 1: Create app
python manage.py startapp [appname]

# STEP 2: Add to INSTALLED_APPS in settings/base.py
INSTALLED_APPS = [
    # ... other apps
    'apps.[appname]',
]

# STEP 3: Create models.py with at least one Page model

# STEP 4: Create templates/[appname]/ directory
```

### Task: Deploy to Production
```bash
# STEP 1: Ensure fly.toml exists

# STEP 2: Set secrets
fly secrets set SECRET_KEY="[generated-key]"
fly secrets set DATABASE_URL="[postgres-url]"

# STEP 3: Deploy
fly deploy

# STEP 4: Run migrations
fly ssh console -C "python manage.py migrate"
```

## ENVIRONMENT VARIABLES

### Development (.env)
```bash
DEBUG=True
SECRET_KEY=dev-secret-key-change-in-production
DATABASE_URL=postgres://user:pass@localhost:5432/springfield
ALLOWED_HOSTS=localhost,127.0.0.1,.gitpod.io,.ona.dev
```

### Production (.env.production)
```bash
DEBUG=False
SECRET_KEY=[generate-secure-key]
DATABASE_URL=[fly-postgres-url]
ALLOWED_HOSTS=springfieldacademy.edu,www.springfieldacademy.edu
AWS_ACCESS_KEY_ID=[cloudflare-r2-key]
AWS_SECRET_ACCESS_KEY=[cloudflare-r2-secret]
AWS_STORAGE_BUCKET_NAME=springfield-media
AWS_S3_ENDPOINT_URL=https://[account-id].r2.cloudflarestorage.com
```

## GOTCHAS AND SOLUTIONS

### Problem: ImportError with Wagtail
```python
# WRONG
from wagtail.core.models import Page
from wagtail.admin.edit_handlers import FieldPanel

# CORRECT (Wagtail 7+)
from wagtail.models import Page
from wagtail.admin.panels import FieldPanel
```

### Problem: StreamField not saving
```python
# WRONG
content = StreamField([...])

# CORRECT
content = StreamField([...], use_json_field=True)
```

### Problem: Settings not found
```python
# WRONG
from django.conf import settings

# CORRECT (in Wagtail context)
from wagtail.contrib.settings.models import BaseSetting, register_setting

@register_setting
class SiteSettings(BaseSetting):
    pass
```

### Problem: Media files not serving
```python
# In settings/base.py
MEDIA_URL = '/media/'
MEDIA_ROOT = os.path.join(BASE_DIR, 'media')

# In urls.py
from django.conf import settings
from django.conf.urls.static import static

urlpatterns = [
    # ... your patterns
] + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```

## PHASE-SPECIFIC CONTEXT

### If Phase 0 (Current)
- Focus: Template evaluation
- Do NOT implement complex features yet
- Priority: Admissions CTAs and basic Program model
- Test both News Template and Starter Kit approaches

### If Phase 1
- Focus: Complete CMS implementation
- All models must be production-ready
- Content migration is priority
- No AI/RAG features yet

### If Phase 2
- RAG integration is OPTIONAL
- Runs as separate service
- Falls back to Wagtail search if unavailable

## COMMANDS REFERENCE

### Most Used Commands
```bash
# Development
python manage.py runserver
python manage.py makemigrations
python manage.py migrate
python manage.py createsuperuser
python manage.py test

# Tailwind CSS
npx tailwindcss -i ./static/css/input.css -o ./static/css/output.css --watch

# Git workflow
git add .
git commit -m "feat: [description]"  # Use conventional commits
git push origin main

# Fly.io deployment
fly deploy
fly logs -a springfield-academy
fly ssh console
```

## TESTING REQUIREMENTS

### Every PR Must Pass
1. `python manage.py test` - All unit tests
2. Lighthouse mobile score ≥ 85
3. No accessibility errors (axe DevTools)
4. Forms send emails successfully
5. Admissions CTAs visible on all pages

## DEPENDENCIES TO NEVER REMOVE

```python
# requirements/base.txt
Django>=5.0,<5.1
wagtail>=7.0,<7.1  # Updated to match global tech stack
psycopg2-binary
django-environ  # For environment variables
django-taggit  # For tagging
modelcluster  # For ParentalKey relationships
beautifulsoup4  # For HTML processing
Pillow  # For image handling
gunicorn  # For production server
```

## AI ASSISTANT INSTRUCTIONS

When generating code for this project:

1. ALWAYS use the exact import patterns shown above
2. ALWAYS include admissions CTAs in templates  
3. ALWAYS add use_json_field=True to StreamFields
4. ALWAYS include SEO fields in page models
5. NEVER use deprecated Wagtail imports
6. NEVER forget to add apps to INSTALLED_APPS
7. NEVER create models without migrations
8. PREFER composition over inheritance
9. PREFER StreamField over rigid field structures
10. PREFER Tailwind classes over custom CSS

## QUESTIONS TO ASK BEFORE CODING

Before implementing any feature, confirm:
1. Which phase are we in? (0, 1, 2, or 3)
2. Is this feature required for current phase?
3. Does this need to work on mobile?
4. Should this integrate with admissions flow?
5. Will this need Chinese translation later?

---

# END OF CONTEXT

Use this context for all Springfield Academy CMS development tasks. If information seems outdated or contradictory, prefer the patterns shown in this document.
