## Fonzie: what & why


### Fonzie: what?

<i class="fas fa-cloud fa-2x"></i>

> A generalist REST API for Open edX objects (CRUD).


### Fonzie: why?

<i class="fas fa-tachometer-alt fa-2x"></i>

We want dashboards, back-offices, etc.

More generally, we want new interfaces with a small scope (Single Page Apps).


## A proof-of-concept for
### third-party app development
#### with
## Docker


## Container-native development

```
fonzie/
├── AUTHORS
├── CHANGELOG.rst
├── ...
├── .circleci/config.yml
├── ...
├── docker-compose.yml
├── Dockerfile
├── Dockerfile-dev
├── ...
├── LICENSE.txt
├── ...
├── README.rst
└── ...
```


## Open edX + Docker = <3

```Dockerfile
# Dockerfile
FROM fundocker/edxapp:ginkgo.1-1.0.3

# Override Open edX LMS settings and URLs
COPY ./edx-platform/config/lms/docker_run.py \
        /config/lms/docker_run.py
COPY ./edx-platform/lms/urls.py \
        /edx/app/edxapp/edx-platform/lms/urls.py

# Copy application sources
COPY . /app/fonzie/

# Install application and project requirements
RUN cd /app/fonzie && \
    pip install -r requirements.txt
```


## Open edX + Docker = <3

```yaml
# docker-compose.yml
version: 3.2
services:
    mysql: # [...]
    mongodb: # [...]
    lms:
        build: .
        environment:
            SERVICE_VARIANT: lms
            DJANGO_SETTINGS_MODULE: lms.envs.fun.docker_run
        depends_on:
            - mysql
            - mongodb
            - # [...]

```

```bash
$ docker-compose up -d lms # 🚀
```


### API specification

<i class="far fa-edit fa-2x"></i>

* Should be the first step
* **Back-end**: automate API implementation testing
* **Front-end**: provide a mock server


### [api blueprint](https://apiblueprint.org)

```markdown
<!-- fonzie-v1-0.apib / [...] -->
## Version [/status/version]

### Retrieve API version (SemVer string) [GET]

+ Request
    + Headers

            Accept: application/json

+ Response 200 (application/json)

        {
            "version": "0.1.0"
        }

```


## Community-driven approach

<i class="fas fa-people-carry fa-2x"></i>

* Collaborative specification (github issues, PR)
* Define priorities in endpoints implementation
* Incremental growth of the API

<i class="fab fa-github"></i>
[github.com/openfun/fonzie](https://github.com/openfun/fonzie)
