
# Node.JS + MySQL Web App.<br><br>Monitoring logs of the app using ELK stack.

<br>

> **Note:** This is a part of a series of demo projects in which I manipulate a Node.js application using various technologies.<br>
>
> The app built using Node.js and Express, originally presented at this [GitHub Repository](https://github.com/otam-mato/nodejs_mysql_web_app_terraform.git).
>
> In the current installment, I am setting up monitoring of the application logs using ['bunyan' library](https://www.npmjs.com/package/bunyan-logger) and ELK stack (Elastic search, Logstash, Kibana).

<br>

## Deployment Strategy

1. The app deployed on the AWS EC2 instance. The app configuration modified to employ ['bunyan logger'](https://www.npmjs.com/package/bunyan-logger) for logging.

2. ELK stack deployed within three containers using 'Docker-compose'.

3. MySQL database deployed as a Docker container.

<br>

## Technologies used
- **ELK stack (Elastic search, Logstash, Kibana)**
- **Node.JS**
- **Bunyan Logger**
- **Express**
- **JavaScript**
- **MySQL**
- **Docker, Docker-compose**
- **AWS**
- **EC2**
<br>

## Application Functionality

This web application interfaces with a MySQL database, facilitating CRUD (Create, Read, Update, Delete) operations on the database records.

**<details markdown=1><summary markdown="span">Detailed app description</summary>**

## Summary

The app sets up a web server for a supplier management system. It allows viewing, adding, updating, and deleting suppliers. 

#### **Dependencies and Modules**:
   - **express**: The framework that allows us to set up and run a web server.
   - **body-parser**: A tool that lets the server read and understand data sent in requests.
   - **cors**: Ensures the server can communicate with different web addresses or domains.
   - **mustache-express**: A template engine, letting the server display dynamic web pages using the Mustache format.
   - **serve-favicon**: Provides the small icon seen on browser tabs for the website.
   - **Custom Modules**: 
     - `supplier.controller`: Handles the logic for managing suppliers like fetching, adding, or updating their details.
     - `config.js`: Keeps the server's settings for connectind to the MySQL database.

#### **Configuration**:
   - The server starts on a port taken from a setting (like an environment variable) or uses `3000` as a default.

#### **Middleware**:
   - It's equipped to understand data in JSON format or when it's URL-encoded.
   - It can chat with web pages hosted elsewhere, thanks to CORS.
   - Mustache is the chosen format for web pages, with templates stored in a folder named `views`.
   - There's a public storage (`public`) for things like images or stylesheets, accessible by anyone visiting the site.
   - The site's tiny browser tab icon is fetched using `serve-favicon`.

#### **Routes (Webpage Endpoints)**:
   - **Home**: `GET /`: Serves the home page.
   - **Supplier Operations**: 
     - `GET /suppliers/`: Fetches and displays all suppliers.
     - `GET /supplier-add`: Serves a page to add a new supplier.
     - `POST /supplier-add`: Receives data to add a new supplier.
     - `GET /supplier-update/:id`: Serves a page to update details of a supplier using its ID.
     - `POST /supplier-update`: Receives updated data of a supplier.
     - `POST /supplier-remove/:id`: Removes a supplier using its ID.

#### **Starting Up**:
   - The server comes to life, starts listening for visits, and announces its awakening with a log message.

</details>

<br>

<details markdown=1><summary markdown="span">Details of the Logging solution</summary>

## Summary

The implemented logging solution maintains a detailed diary (logs) of everything that happens, ensuring transparency and traceability. It is based on 'Bunyan logger'.

### The Logging Solution:

#### **Setting up and Distinguishing Logs**:
   - A special logger (`appLogger`) ensures that all the server's actions are noted down separately from other potential system logs. In my case, I want to sepatrate `app` logs from `config` logs.
     - Configuration-related logs will be marked with source: 'config'.
     - All other application-related logs will be marked with source: 'app'.
   This way, by looking at the log source, you can easily identify where the log came from.

#### **Capturing All Messages**:
   - The common ways developers use to write logs (like just printing messages) are tweaked. Now, instead of only printing, they make sure messages are properly recorded using `appLogger`.

#### **Monitoring Requests**:
   - Every time someone interacts with the server, `logRequests` notes down what they did and which part of the server they accessed.

#### **Server Activities**:
   - Be it starting up, or any supplier-related activity like viewing, adding, or deleting, everything gets its own log entry.

</details>
