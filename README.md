# Playground for PM thread exports to WBB

This repository contains everything necessary to create:

* A single MyBB 1.8.38 installation, based on PHP 8.2
* ...with two users: _admin_ and _user_
* ...who have exchanged five PMs in total
* ...which should get rendered into a single thread when imported into WBB
* A phpMyAdmin installation for quick database inspections

## Requirements

* Docker CE
* Docker-Compose V2
* Unused ports `:8080` and `:8081`

## Getting Started

Since MyBB does *not* provide a PHP 8.2 based image, we build it ourselves.
For that to work, we have created our own `Dockerfile`, but we still use the regular process that MyBB uses themselves.

You need to make sure that the `docker/` submodule is pulled and up to date (this only needs to be done once):

```shell
$ git submodule update --init
```

Now, just run the following command and wait for a couple seconds:

```shell
$ docker-compose up -d
```

MyBB will download a copy of itself into the `wwwroot/` folder, that is wholly ignored in `git`.

The MyBB Installation should then be available at [`http://localhost:8080`](http://localhost:8080).

### Credentials

| Username | Password | Privileges      |
|----------|----------|-----------------|
| admin    | password | Administrator   |
| user     | password | Registered User |

Both credentials work just fine in the forum, but only the `admin` credentials work in
the [ACP](http://localhost:8080/acp).

### Re-enable the Default Theme

Due to the immutable nature of this approach, we don't save away the caches, which makes the theme
look rather jarring without CSS and other assets.

If you want to have the "full experience", re-enable the theme like so:

1. Enter the [`Templates & Styles > Themes`](http://localhost:8080/admin/index.php?module=style-themes) area in the ACP.
2. Edit the `Default` theme.
3. Just leave the page.

The theme should now display properly.

You can also use [this direct link](http://localhost:8080/admin/index.php?module=style-themes&action=edit&tid=2) to edit
the theme in question.

## phpMyAdmin

Launch phpMyAdmin by browsing to [`http://localhost:8081`](http://localhost:8081).

MyBB is installed into the `mybb` database.

## Installing WBB

To install WoltLab Burning Board for a migration test run, create a new folder inside `wwwroot/` and work away.

