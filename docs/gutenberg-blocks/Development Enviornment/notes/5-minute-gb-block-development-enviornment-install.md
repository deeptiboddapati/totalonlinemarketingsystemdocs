---
sidebar_label: '5 Min Gutenberg Block Development Environment Install'
sidebar_position: 1
---

# Block Development Enviornment

## Development Enviornment Quickstart
1. [Run docker Desktop](#run-docker-desktop)
2. [Clone the repository](#clone-the-repository)
3. [Set up the .env file](#3-set-up-the-env-file)
4. [Run docker compose up](#run-docker-compose-up)

### 1. Run Docker Desktop
Visit [the docker desktop download page.](https://www.docker.com/products/docker-desktop/) Download the latest Docker Desktop executable. Run it on your PC.

### 2. Clone the repository

The repository is hosted at [link](#repository). It contains the plugin(s) that contains all the custom Gutenberg blocks.
Clone it.

### 3. Set up the .env file
Copy the .env.example file, change the name of the copy to .env. Edit the .env file with custom values for all the variables.

### 4. Run Docker Compose Up
Navigate to the directory in a terminal. Run docker compose up.


## WP CLI
docker-compose run --rm wpcli
https://blog.sixeyed.com/your-must-have-powershell-aliases-for-docker/