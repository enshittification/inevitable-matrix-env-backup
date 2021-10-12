# matrix-env
This project provides a self-contained [Matrix.org](https://matrix.org) sandbox, using [Docker Compose](https://docs.docker.com/compose). It allows you to quickly get a Matrix _node_ running on your local machine, for exploration or hacking on the Matrix ecosystem.

It gives you the following pre-configured services:

| Service  | Description | URL |
| ------------- | ------------- | ------------- |
| [synapse](https://github.com/matrix-org/synapse) | The reference homeserver implementation | [localhost:8008](http://localhost:8008) |
| [synapse-admin](https://github.com/Awesome-Technologies/synapse-admin) | Homeserver admin UI | [localhost:8009](http://localhost:8009) |
| [element](https://github.com/vector-im/element-web) | Web-based Matrix client | [localhost:8010](http://localhost:8010) |
| [dimension](https://dimension.t2bot.io/) | Integration manager | [localhost:8011](http://localhost:8011) |
| [go-neb](https://github.com/matrix-org/go-neb) | Extensible bot, written in Go | [localhost:8012](http://localhost:8012) |
| [maubot](https://github.com/maubot/maubot) | Extensible bot, written in Python | [localhost:8013](http://localhost:8013) |

## Instructions
Create the `.env.local` file, which you can use to override environment variables defined in `.env`, if you so wish:

```shell
echo "# Environment variables defined in this file override the ones defined in .env" > .env.local
```

Then start everything with:

```shell
docker compose up
```

> You might notice Dimension exits with an error. This is expected, since Dimension requires [further configuration](#configuring-dimension) to properly boot. However, if you don't plan on using Dimension, you can safely ignore this error.

## Creating users
There are no pre-configured users, and registration through the client is disabled. Before you can login, you must create a user. To do so, you can use the [bin/register_new_matrix_user](bin/register_new_matrix_user) command:

```shell
# Create a privileged user with username 'admin' and password 'admin'.
# Add --help to see documentation.

bin/register_new_matrix_user -u admin -p admin --admin
```

## Configuring Dimension
Out of the box, Dimension (the integration manager) will not properly start since it requires further configuration. If you intend to use Dimension,  please follow [these instructions](dimension.md).

## Database access
Each service uses its own SQLite database. To access each database, simply open the file with an SQLite client. The files are stored under:

- synapse: `synapse/data/homeserver.db`
- dimension: `dimension/dimension.db`
- maubot: `maubot/maubot.db`
- go-neb: `go-neb/go-neb.db`

## Credentials

- Matrix users
    - admin:
        - username: `admin`
        - password: `admin`
    - dimension:
        - username: `dimension`
        - password: `dimension`
- maubot
    - username: `admin`
    - password: `admin`

## Starting from scratch
Use the following commands to remove all containers and all data:

```shell
docker compose down
rm synapse/data/homeserver.db dimension/dimension.db maubot/maubot.db go-neb/go-neb.db
```

Also note that Element stores data in the browser's local storage. To really start from scratch, you must also delete all browser data related to http://localhost:8010.
