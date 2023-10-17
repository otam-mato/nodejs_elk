
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
