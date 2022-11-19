# web-dev-docker

Docker based environments for web development.

<img width="1214" alt="image" src="https://user-images.githubusercontent.com/1764695/202864412-f10c628c-00fb-4484-8059-7a577a54ebac.png">

## Systems

### PHP

1. PHP 7.4
2. PHP 8.0
3. PHP 8.1
4. PHP 8.2 RC6

### Symfony

1. Symfony 4.4 LTS
2. Symfony 5.4 LTS
3. Symfony 6.0
4. Symfony 6.1 (current)
5. Symfony 6.2 (dev)

### TYPO3

1. TYPO3 v9
2. TYPO3 v10
3. TYPO3 v11
4. TYPO3 v12 (no auto-install yet)

### WordPress

1. Wordpress (current)

### Stack

1. NGINX
2. MariaDB
3. Redis
4. xDebug

## Installation

### Release

```bash
unzip =(wget -qO- https://github.com/CTH24/web-dev-docker/archive/refs/tags/v1.0.0.zip) \
  && mv web-dev-docker-1.0.0 project-name
```

## Commands

```bash
./start
```

Complete system reset. All Files and Databases will be deleted.

```bash
./reset
```

## MySQL

```
HOST: db
DATABASE: website
USER: website
PASS: website
```
