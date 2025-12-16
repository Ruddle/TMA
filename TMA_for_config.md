# TMA for Config Files

This document provides varied examples of how TMA (Text Markup) can represent common configuration file formats. TMA's simplicity and convention-based approach make it suitable for many configuration scenarios.

## 1. INI-style Configuration

Traditional INI file:
```ini
[database]
host = localhost
port = 3306
username = admin
password = secret

[server]
host = 0.0.0.0
port = 8080
debug = true
```

TMA representation:
```c++
(
  database(
    host(localhost)
    port(3306)
    username(admin)
    password(secret)
  )
  server(
    host(0.0.0.0)
    port(8080)
    debug(true)
  )
)
```

## 2. YAML Configuration

YAML file (Docker Compose example):
```yaml
version: '3.8'
services:
  web:
    image: nginx:alpine
    ports:
      - "80:80"
    environment:
      - NODE_ENV=production
  db:
    image: postgres:13
    environment:
      POSTGRES_PASSWORD: example
```

TMA representation (two possible conventions):
```c++
// Convention 1: Preserve array structure
(
  version(3.8)
  services(
    web(
      image(nginx:alpine)
      ports((80:80))
      environment(
        (NODE_ENV(production))
      )
    )
    db(
      image(postgres:13)
      environment(
        POSTGRES_PASSWORD(example)
      )
    )
  )
)

// Convention 2: Flatten environment variables
(
  version(3.8)
  services(
    web(
      image(nginx:alpine)
      ports((80:80))
      environment(
        NODE_ENV(production)
      )
    )
    db(
      image(postgres:13)
      environment(
        POSTGRES_PASSWORD(example)
      )
    )
  )
)
```

## 3. TOML Configuration

TOML file (Rust Cargo.toml example):
```toml
[package]
name = "my_project"
version = "0.1.0"
edition = "2021"

[dependencies]
serde = "1.0"
tokio = { version = "1.0", features = ["full"] }

[profile.release]
opt-level = 3
```

TMA representation:
```c++
(
  package(
    name(my_project)
    version(0.1.0)
    edition(2021)
  )
  dependencies(
    serde(1.0)
    tokio(version(1.0) features((full)))
  )
  profile(
    release(
      opt-level(3)
    )
  )
)
```

## 4. Environment Variables (.env)

.env file:
```env
DATABASE_URL=postgres://user:pass@localhost/db
API_KEY=sk_test_1234567890
DEBUG=true
MAX_CONNECTIONS=10
```

TMA representation:
```c++
(
  DATABASE_URL(postgres://user:pass@localhost/db)
  API_KEY(sk_test_1234567890)
  DEBUG(true)
  MAX_CONNECTIONS(10)
)
```

## 5. XML Configuration

XML web.config example:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <system.web>
    <compilation debug="true" targetFramework="4.8" />
    <httpRuntime targetFramework="4.8" />
  </system.web>
  <appSettings>
    <add key="ApplicationName" value="MyApp" />
  </appSettings>
</configuration>
```

TMA representation:
```c++
configuration(
  system.web(
    compilation(debug(true) targetFramework(4.8))
    httpRuntime(targetFramework(4.8))
  )
  appSettings(
    add(key(ApplicationName) value(MyApp))
  )
)
```

## 6. JSON Configuration

package.json example:
```json
{
  "name": "my-app",
  "version": "1.0.0",
  "scripts": {
    "start": "node index.js",
    "test": "jest"
  },
  "dependencies": {
    "express": "^4.18.0",
    "lodash": "^4.17.21"
  }
}
```

TMA representation:
```c++
(
  name(my-app)
  version(1.0.0)
  scripts(
    start(node index.js)
    test(jest)
  )
  dependencies(
    express(^4.18.0)
    lodash(^4.17.21)
  )
)
```

## 7. Nginx Configuration

Nginx config snippet:
```nginx
server {
    listen 80;
    server_name example.com;
    
    location / {
        root /var/www/html;
        index index.html;
    }
    
    location /api {
        proxy_pass http://localhost:3000;
    }
}
```

TMA representation:
```c++
server(
  listen(80)
  server_name(example.com)
  location(
    path(/)
    root(/var/www/html)
    index(index.html)
  )
  location(
    path(/api)
    proxy_pass(http://localhost:3000)
  )
)
```

## 8. Git Configuration

.gitconfig example:
```gitconfig
[user]
    name = John Doe
    email = john@example.com
[core]
    editor = vim
    autocrlf = input
[alias]
    co = checkout
    br = branch
    ci = commit
```

TMA representation:
```c++
(
  user(
    name(John Doe)
    email(john@example.com)
  )
  core(
    editor(vim)
    autocrlf(input)
  )
  alias(
    co(checkout)
    br(branch)
    ci(commit)
  )
)
```

## 9. Simple Key-Value Pairs

Simple configuration:
```properties
theme=dark
language=en_US
timezone=America/New_York
items_per_page=25
```

TMA representation:
```c++
(
  theme(dark)
  language(en_US)
  timezone(America/New_York)
  items_per_page(25)
)
```

## 10. Hierarchical Application Configuration

Complex application config:
```yaml
app:
  name: "My Application"
  version: "2.1.0"
  features:
    - authentication
    - notifications
    - analytics
  database:
    primary:
      host: "db1.example.com"
      port: 5432
    replica:
      host: "db2.example.com"
      port: 5432
```

TMA representation:
```c++
app(
  name(My Application)
  version(2.1.0)
  features(
    (authentication)
    (notifications)
    (analytics)
  )
  database(
    primary(
      host(db1.example.com)
      port(5432)
    )
    replica(
      host(db2.example.com)
      port(5432)
    )
  )
)
```
