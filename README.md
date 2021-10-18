# Explanation

1. When you load up the app it will generate a link that you can click on. This is done by using the "/api/create_link_token" end point.
2. After you click on the link it will generate a public_token using the "/api/set_access_token" endpoint.
3. Link flow uses that public_token to generate a access_token and a item_id. The access_token and item_id are then used to make calls to retrieve other user data(transactions, indentity, balance etc...)
   [Link flow](https://plaid.com/docs/link/) is a client side UI that you can us to link your accounts to Plaid, so that you can access the relavent information using the Plaid API

For example if you want to receive all the accounts you would use the following method:

    app.get('/api/accounts', async function (request, response, next) {
      try {
        const accountsResponse = await client.accountsGet({
          access_token: accessToken,
        });
        prettyPrintResponse(accountsResponse);
        response.json(accountsResponse.data);
      } catch (error) {
        prettyPrintResponse(error);
        return response.json(formatError(error.response));
      }
    });

It is was hard for me to use Insomnia to retrieve the data becuase I first needed to have a access_token. One way I did manage to get information back, was after I logged data inside frontend/index and by using the access_token from the console.log and [this method](https://documenter.getpostman.com/view/4675947/RWMLHkHR#64d97a2b-071b-4826-838c-a8acec7d33ec) I was able to get the list of transactions. (/frontend/pictures/Capture.PNG)

It is possible to use the website to login and view the relavent information.

1. you need to search for oauth bank and select the first Platypus OAuth Bank.
2. The userName and password is user_good for both
3. Since this is a test run you dont need a mobile or a verification code
4. after selecting an account you can make requests and see the relavent info

# Plaid quickstart

This repository accompanies Plaid's [**quickstart guide**][quickstart].

Here you'll find full example integration apps using our [**client libraries**][libraries]:

![Plaid quickstart app](/assets/quickstart.jpeg)

## Table of contents

<!-- toc -->

- [1. Clone the repository](#1-clone-the-repository)
  - [Special instructions for Windows](#special-instructions-for-windows)
- [2. Set up your environment variables](#2-set-up-your-environment-variables)
- [3. Run the quickstart](#3-run-the-quickstart)
  - [Run with Docker](#run-with-docker)
    - [Pre-requisites](#pre-requisites-1)
    - [Running](#running-1)
      - [Start the container](#start-the-container)
      - [View the logs](#view-the-logs)
      - [Stop the container](#stop-the-container)
  - [Run without Docker](#run-without-docker)
    - [Pre-requisites](#pre-requisites)
    - [1. Running the backend](#1-running-the-backend)
      - [Node](#node)
      - [Python](#python)
      - [Ruby](#ruby)
      - [Go](#go)
      - [Java](#java)
    - [2. Running the frontend](#2-running-the-frontend)
- [Testing OAuth](#testing-oauth)

<!-- tocstop -->

## 1. Clone the repository

Using https:

```
$ git clone https://github.com/plaid/quickstart
$ cd quickstart
```

Alternatively, if you use ssh:

```
$ git clone git@github.com:plaid/quickstart.git
$ cd quickstart
```

#### Special instructions for Windows

Note - because this repository makes use of symbolic links, to run this on a Windows machine, make sure you have checked the "enable symbolic links" box when you download Git to your local machine. Then you can run the above commands to clone the quickstart. Otherwise, you may open your Git Bash terminal as an administrator and use the following command when cloning the project

```
$ git clone -c core.symlinks=true https://github.com/plaid/quickstart
```

## 2. Set up your environment variables

```
$ cp .env.example .env
```

Copy `.env.example` to a new file called `.env` and fill out the environment variables inside. At
minimum `PLAID_CLIENT_ID` and `PLAID_SECRET` must be filled out. Get your Client ID and secrets from
the dashboard: https://dashboard.plaid.com/account/keys

> NOTE: `.env` files are a convenient local development tool. Never run a production application
> using an environment file with secrets in it.

## 3. Run the Quickstart

There are two ways to run the various language quickstarts in this repository. You can simply choose to use Docker, or you can run the
code directly. If you would like to run the code directly, skip to the
[Run without Docker](#run-without-docker) section.

### Run with Docker

#### Pre-requisites

- `make` available at your command line
- Docker installed and running on your machine: https://docs.docker.com/get-docker/
- Your environment variables populated in `.env`
- If using Windows, a working Linux installation on Windows 10. If you are using Windows and do not already have WSL or Cygwin configured, we recommend [running without Docker](#run-without-docker).

#### Running

There are three basic `make` commands available

- `up`: builds and starts the container
- `logs`: tails logs
- `stop`: stops the container

Each of these should be used with a `language` argument, which is one of `node`, `python`, `ruby`,
`java`, or `go`. If unspecified, the default is `node`.

##### Start the container

```
$ make up language=node
```

The quickstart backend is now running on http://localhost:8000 and frontend on http://localhost:3000.

If you make changes to one of the server files such as `index.js`, `server.go`, etc, or to the
`.env` file, simply run `make up language=node` again to rebuild and restart the container.

If you experience a Docker connection error when running the command above, try the following:

- Make sure Docker is running
- Try running the command prefixed with `sudo`

##### View the logs

```
$ make logs language=node
```

##### Stop the container

```
$ make stop language=node
```

### Run without Docker

#### Pre-requisites

- The language you intend to use is installed on your machine and available at your command line.
  This repo should generally work with active LTS versions of each language such as node >= 14,
  python >= 3.8, ruby >= 2.6, etc.
- Your environment variables populated in `.env`
- [npm](https://www.npmjs.com/get-npm)
- If using Windows, a command line utility capable of running basic Unix shell commands

#### 1. Running the backend

Once started with one of the commands below, the quickstart will be running on http://localhost:8000 for the backend. Enter the additional commands in step 2 to run the frontend which will run on http://localhost:3000.

##### Node

```
$ cd ./node
$ npm install
$ ./start.sh
```

##### Python

**:warning: As `python2` has reached its end of life, only `python3` is supported.**

```
$ cd ./python

# If you use virtualenv
# virtualenv venv
# source venv/bin/activate

$ pip install -r requirements.txt
$ ./start.sh
```

If you get this error message:

```
ssl.SSLError: [SSL: CERTIFICATE_VERIFY_FAILED] certificate verify failed (_ssl.c:749)
```

You may need to run the following command in your terminal for your particular version of python in order to install SSL certificates:

```
# examples:
open /Applications/Python\ 3.9/Install\ Certificates.command
# or
open /Applications/Python\ 3.6/Install\ Certificates.command
```

##### Ruby

```
$ cd ./ruby
$ bundle
$ ./start.sh
```

##### Go

```
$ cd ./go
$ go build
$ ./start.sh
```

##### Java

```
$ cd ./java
$ mvn clean package
$ ./start.sh
```

#### 2. Running the frontend

```
$ cd ./frontend
$ npm install
$ npm start

```

## Testing OAuth

Some institutions (primarily in Europe, but a small number in the US) require an OAuth redirect
authentication flow, where the end user is redirected to the bank’s website or mobile app to
authenticate. For this flow, you should set `PLAID_REDIRECT_URI=http://localhost:3000/` in `.env`.
You will also need to register this localhost redirect URI in the
[Plaid dashboard under Team Settings > API > Allowed redirect URIs][dashboard-api-section].

OAuth flows are only testable in the `sandbox` environment in this Quickstart app due to an https
`redirect_uri` being required in other environments. Additionally, if you want to use the [Payment
Initiation][payment-initiation] product, you will need to [contact Sales][contact-sales] to get this
product enabled.

[quickstart]: https://plaid.com/docs/quickstart
[libraries]: https://plaid.com/docs/api/libraries
[payment-initiation]: https://plaid.com/docs/payment-initiation/
[node-example]: /node
[ruby-example]: /ruby
[python-example]: /python
[java-example]: /java
[go-example]: /go
[docker]: https://www.docker.com
[dashboard-api-section]: https://dashboard.plaid.com/team/api
[contact-sales]: https://plaid.com/contact
