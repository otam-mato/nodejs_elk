
# Node.JS + MySQL Web App.<br><br>Monitoring logs of the app using ELK stack.

<br>

> **Note:** This is a part of a series of demo projects in which I manipulate a Node.js application using various technologies.<br>
>
> The app built using Node.js and Express, originally presented at this [GitHub Repository](https://github.com/otam-mato/nodejs_mysql_web_app_terraform.git).
>
> In the current installment, I am setting up monitoring of the application logs using ['bunyan' library](https://www.npmjs.com/package/bunyan-logger) and ELK stack (Elastic search, Logstash, Kibana).

<br>

## Deployment Strategy

1. Node.js Application Deployment:
   We will deploy the Node.js application on an AWS EC2 instance. As part of this deployment, we will enhance the application's logging capabilities by implementing the 'bunyan logger' for structured logging.

2. ELK Stack Implementation:
   We will employ the ELK (Elasticsearch, Logstash, Kibana) stack, utilizing Docker-compose to pull and run three containers. This setup ensures centralized log collection, processing, and visualization, streamlining troubleshooting and monitoring.

3. MySQL Database Deployment:
   Our MySQL database will be integrated into our infrastructure as a Docker container. 

<br>

## Architecture Diagram

<p align="center">
  <img src="https://github.com/otam-mato/nodejs_elk/assets/113034133/8903b3e4-912c-413f-b162-5a6ffc2fe923" width="800px"/>
</p>

<br>

## Technologies used
- **ELK stack (Elastic search, Logstash, Kibana)**
- **Node.JS**
- **Bunyan Logger**
- **Express**
- **JavaScript**
- **MySQL**
- **Docker, Docker-Compose**
- **AWS**
- **EC2**
<br>

## Functionality

This web application interfaces with a MySQL database, facilitating CRUD (Create, Read, Update, Delete) operations on the database records. Logging is implemented based on **'Bunyan logger'** ensuring structured and reliable recording of application activities, errors, and events. Monitoring of logs integrated via **ELK stack**.

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

**<details markdown=1><summary markdown="span">Details of the Logging solution</summary>**

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

**<details markdown=1><summary markdown="span">Details of the ELK stack</summary>**

**We launch our ELK stack using this [docker-compose file]()**
   
The ELK stack, which stands for Elasticsearch, Logstash, and Kibana, is a popular set of tools for searching, analyzing, and visualizing data in real-time. Over the years, the stack has grown to include Beats and is sometimes referred to as the Elastic Stack, but ELK remains a popular term. Here's a detailed description of each component:

### 1. **Elasticsearch**

**Overview**: 
Elasticsearch is a distributed, RESTful search and analytics engine. It's built on top of Lucene and provides a means of storing, searching, and analyzing large volumes of data quickly and in near real-time.

**Key Features**:
- **Distributed**: Automatically splits and distributes data across multiple nodes. Supports clustering, sharding, and replication.
- **Full-Text Search**: Built on top of the Lucene library, it provides powerful full-text search capabilities.
- **RESTful API**: Interact with the data stored in Elasticsearch using RESTful API endpoints, returning data in JSON format.
- **Schema-Free**: JSON documents can be stored without a predefined schema, but you can define custom mappings.
- **Scalability**: Can scale out by adding more nodes to the cluster. Handles large amounts of data efficiently.

### 2. **Logstash**

**Overview**: 
Logstash is a server-side data processing pipeline that ingests, transforms, and then sends data to the specified destination. It can take in data from various sources, process it, and send it to various outputs.

**Key Features**:
- **Inputs**: Supports various input plugins to gather data, including logs, metrics, and other telemetry.
- **Filters**: Data can be transformed and enriched using a variety of filter plugins. Common operations include parsing log entries, adding geographical data from IP addresses, and changing field names.
- **Outputs**: Supports various output plugins to send data to destinations like Elasticsearch, cloud services, or even local files.
- **Extensibility**: Has a rich collection of plugins and can be extended with custom plugins as well.

### 3. **Kibana**

**Overview**: 
Kibana is the visualization layer of the ELK stack. It provides search and data visualization capabilities for data indexed in Elasticsearch.

**Key Features**:
- **Dashboards**: Create, share, and embed interactive data visualizations and dashboards.
- **Data Exploration**: Offers a user-friendly interface to search and view data stored in Elasticsearch.
- **Advanced Analytics**: Supports advanced time series analysis, machine learning features, graph exploration, and more.
- **Management & Monitoring**: Kibana also comes with tools for managing and monitoring the Elasticsearch cluster, as well as advanced features like alerting.
- **Extensibility**: Can be customized with plugins and offers integrations with other Elastic tools.

### **How They Work Together**:

- **Data Collection & Processing**: Logstash collects data from various sources. In our case, we employ bunyan for that. It then processes, transforms, and enriches the data based on user-defined rules.
  
- **Data Storage**: Once processed, Logstash sends the data to Elasticsearch for storage. Elasticsearch indexes the data, making it quickly searchable.
  
- **Visualization & Analysis**: Kibana interfaces with Elasticsearch to search, view, and visualize the data. Users can create custom dashboards in Kibana to monitor and analyze their data.

Together, the ELK stack provides an end-to-end solution for gathering, processing, storing, and visualizing data, making it a popular choice for log and event data management in particular.

</details>

<br>

## Prerequisites

- A work station or an **EC2** instance (I am using **Ubuntu 22.04**).
   
- <details markdown=1><summary markdown="span">Install Docker-compose, Node.js, npm, and Git </summary>
   <br>
   
   - **Install Docker-compose**

   ```
   sudo apt update
   sudo apt install docker-compose
   ```
   
   - **Install Node.js**

   ```
   sudo apt install nodejs
   ```
   
   - **Install npm**

   ```
   sudo apt install npm
   ```
   
   - **Install Git**

   ```
   sudo apt install git
   ```
   
  </details>

<br>

## Implementation

Follow these steps for successful implementation:

1. [**Modify the app files to employ Bunyan for logging**](https://github.com/otam-mato/nodejs_elk/blob/main/README.md#modify-the-app-files-to-employ-bunyan-for-logging)
2. [**Create the Logstash configuration file**](https://github.com/otam-mato/nodejs_elk/blob/main/README.md#2-create-the-logstash-configuration-file)
3. [**Launch the app**]()
4. [**Launch the Dockerized MySQL server**]()
5. [**Launch Elastic Search, Logstash, Kibana using Docker-Compose manifest**]()
6. [**Set up Kibana (index pattern and dashboard)**]()
7. [**Test the app**]()

<br>

## Screenshots

<p align="center">
  <img src="https://github.com/otam-mato/nodejs_elk/assets/113034133/447326c7-fd05-46a9-9385-42a96e06ba7d" width="700px"/>
</p>

<p align="center">
  <img src="https://github.com/otam-mato/nodejs_elk/assets/113034133/d28a657f-9732-42d9-ab43-62c77f8b503b" width="700px"/>
</p>

<br>

## Steps

### 1. Modify the app files to employ Bunyan for logging

- [**config.js**](https://github.com/otam-mato/nodejs_elk/blob/1296ecd681aa431154f71953f839891f74ca50b2/app/config/config.js)
- [**index.js**](https://github.com/otam-mato/nodejs_elk/blob/10accacf5ef25b4364e0e07a619b878a529d61e0/index.js)

### 2. Create the Logstash configuration file

- [**nodeapp.conf**](https://github.com/otam-mato/nodejs_elk/blob/1a84d36fd5b4b2143a74871e60a6e3b30a3ec8af/nodeapp.conf)

### 3. Launch the ELK stack with this Docker-compose file

- [**docker-compose.yml**](https://github.com/otam-mato/nodejs_elk/blob/df0d9df49a571fefb756d48106e63a51d52e01d1/docker-compose.yml)

  
```
docker-compose up
```

### 4. To start the backend (mysql database) pull and run this image from DockerHub:

```
docker pull montcarotte/fullstack_nodejs_mysql_demo:mysql_server
```

<img width="700" alt="S" src="https://github.com/otammato/6_ELK_monitoring_NodeJS_logs/assets/104728608/3a67f57c-66c9-45a0-a3b5-97b07c554106">

<br>

```
docker run --name mysql_1 -p 3306:3306 -d mysql_server
```

<br>

### 5. Launch the app and click a few times at any links to start producing logs.

- **start the app:**

```js
node index.js
```
- **access the app via port 3000:**

<img width="700" alt="S" src="https://github.com/otam-mato/nodejs_elk/assets/113034133/7a5a54d9-535e-44d1-b91f-d589795523ba">

- **check the logs being produced:**

  As one of the routes suggests keeping logs in a file, check [logs.log](https://github.com/otam-mato/nodejs_elk/blob/c598480951240597d07810e2eff4464b7bdf8d91/logs.log)

<br>

### 7. Set up kibana (set up index pattern and a dashboard)

<img width="700" alt="Screenshot 2023-08-21 at 22 23 27" src="https://github.com/otammato/ELK_monitoring_NodeJS_logs/assets/104728608/d8bea94a-3de3-4614-a172-0fea507a1994">
<img width="700" alt="Screenshot 2023-08-21 at 22 24 01" src="https://github.com/otammato/ELK_monitoring_NodeJS_logs/assets/104728608/efaa83da-1589-4737-9b2c-f5652b5c7e6d">
<img width="700" alt="Screenshot 2023-08-21 at 22 24 38" src="https://github.com/otammato/ELK_monitoring_NodeJS_logs/assets/104728608/a750f1bc-6d5c-4203-be1a-ed6bd11c74a8">
<img width="700" alt="Screenshot 2023-08-21 at 22 25 47" src="https://github.com/otammato/ELK_monitoring_NodeJS_logs/assets/104728608/681b3406-f5a8-4e0d-bf3d-f7457e33a778">
<img width="700" alt="Screenshot 2023-08-21 at 22 26 04" src="https://github.com/otammato/ELK_monitoring_NodeJS_logs/assets/104728608/465dd78a-3c17-4fd4-bcc6-2cb5347375e5">



<img width="700" alt="Screenshot 2023-08-21 at 22 25 07" src="https://github.com/otammato/ELK_monitoring_NodeJS_logs/assets/104728608/067dccea-11cd-49f7-b61c-efdc6c588e4c">
<img width="700" alt="Screenshot 2023-08-21 at 22 25 26" src="https://github.com/otammato/ELK_monitoring_NodeJS_logs/assets/104728608/95e1e230-56c7-40fa-9417-b45559421d51">
<img width="700" alt="Screenshot 2023-08-21 at 22 25 37" src="https://github.com/otammato/ELK_monitoring_NodeJS_logs/assets/104728608/a50b9bac-aba0-4492-ba0b-d677bcd98826">

<img width="700" alt="Screenshot 2023-08-21 at 22 24 24" src="https://github.com/otammato/ELK_monitoring_NodeJS_logs/assets/104728608/56b8f390-011f-469c-b756-69ee23829557">

<br><br>

### 7. Result

- **logs are being collected and ready for further analyzing**

<img width="700" alt="Screenshot 2023-08-21 at 22 32 05" src="https://github.com/otammato/ELK_monitoring_NodeJS_logs/assets/104728608/a7b6560f-a7bd-47a2-8133-1eca1d896830">

<br><br>

- **the example of a dashboard:**

<img width="700" alt="Screenshot 2023-08-23 at 11 34 11" src="https://github.com/otammato/ELK_monitoring_NodeJS_logs/assets/104728608/c6b07918-2887-4f00-987b-5d2184ebab04">
