# AWS Cloud Web Deployment

A beginner-friendly static website deployment project using **AWS EC2,
Amazon Linux, Nginx, Git, GitHub, HTML, and CSS**.

## Project Overview

This project demonstrates how to store website code in GitHub, deploy it
to an AWS EC2 Linux server, and serve the website using Nginx.

The website displays a simple cloud project landing page with the
heading **"Welcome to My Cloud Project"**.

## Live Website

-   **GitHub repository:**
    https://github.com/nandini-cloudops/aws-cloud-web-deployment
-   **EC2 website:** `http://YOUR-EC2-PUBLIC-IP`

Replace `YOUR-EC2-PUBLIC-IP` with the current Public IPv4 address of
your EC2 instance. The public IP can change if the instance is stopped
and started again, unless an Elastic IP is configured.

## Technologies Used

-   **AWS EC2** --- virtual Linux server
-   **Amazon Linux** --- server operating system
-   **Nginx** --- web server that serves the website
-   **Git and GitHub** --- source-code version control and hosting
-   **HTML** --- website structure and content
-   **CSS** --- website styling

## Architecture

``` text
User's Browser
      |
      | HTTP request (port 80)
      v
Internet
      |
      v
AWS EC2 Instance (Amazon Linux)
      |
      v
Nginx Web Server
      |
      v
/usr/share/nginx/html/
      |
      +--> index.html
      +--> style.css
```

## Repository Files

-   `index.html` --- main webpage
-   `style.css` --- webpage styles
-   `aws-cloud-architecture.png` --- project architecture diagram
-   `README.md` --- project documentation

## Deployment Steps

These steps are for an Amazon Linux EC2 instance. Run the commands in
your SSH/PuTTY terminal.

### 1. Connect to EC2

Connect to your running EC2 instance using SSH or PuTTY. The default
username for Amazon Linux is commonly `ec2-user`. Use the correct
username and private key for your instance.

### 2. Install Git and Nginx

For Amazon Linux 2023:

``` bash
sudo dnf update -y
sudo dnf install git nginx -y
```

### 3. Download the project from GitHub

``` bash
cd ~
git clone https://github.com/nandini-cloudops/aws-cloud-web-deployment.git
cd aws-cloud-web-deployment
ls -la
```

If the project directory already exists, use
`cd ~/aws-cloud-web-deployment` instead of cloning it again.

### 4. Copy the website files to the Nginx web directory

For this repository's root-level `index.html` and `style.css`:

``` bash
sudo cp index.html style.css /usr/share/nginx/html/
```

If you later add images or other assets used by the website, copy those
assets to the web directory too, preserving their folder structure.

### 5. Start and enable Nginx

``` bash
sudo systemctl enable --now nginx
sudo systemctl status nginx --no-pager
```

The service status should show `active (running)`.

### 6. Configure the EC2 Security Group

Add an inbound rule to the instance's Security Group:

  Type   Protocol     Port Source
  ------ ---------- ------ -------------
  HTTP   TCP            80 `0.0.0.0/0`

Keep SSH (port 22) restricted to your own IP address where possible. Do
not open SSH to everyone unless you have a specific, temporary reason.

### 7. Open the website

Find the instance's current **Public IPv4 address** in the AWS EC2
console, then open:

``` text
http://YOUR-EC2-PUBLIC-IP
```

## Verification Commands

Check Nginx:

``` bash
sudo systemctl status nginx --no-pager
```

Check the website response from the server:

``` bash
curl -I http://localhost
```

A response such as `HTTP/1.1 200 OK` indicates that the web server
responded successfully.

Check the deployed files:

``` bash
ls -la /usr/share/nginx/html/
```

Compare the repository files with the deployed HTML and CSS:

``` bash
cmp ~/aws-cloud-web-deployment/index.html /usr/share/nginx/html/index.html
cmp ~/aws-cloud-web-deployment/style.css /usr/share/nginx/html/style.css
```

If `cmp` prints nothing and returns to the prompt, the compared files
are identical.

## Security and Cost Notes

-   The website uses HTTP in this basic lab. For a public production
    website, configure HTTPS with a domain name and a TLS certificate.
-   Keep your private key secure. Never upload `.pem` or `.ppk`
    private-key files to GitHub.
-   Restrict SSH access to your own IP address where possible.
-   AWS usage may incur charges. Check your account's pricing and
    billing, and stop or terminate resources when you no longer need
    them. Terminating an instance can permanently delete data stored
    only on its instance storage.

## Learning Outcomes

By completing this project, you practise:

-   Creating and accessing an EC2 Linux server
-   Connecting to a server with SSH/PuTTY
-   Downloading source code from GitHub using Git
-   Installing and managing Nginx with `systemctl`
-   Deploying static HTML/CSS files
-   Configuring an EC2 Security Group for HTTP
-   Testing a web server with `curl`

## Author

**Nandini Dhanasure**

------------------------------------------------------------------------

*Educational project for learning AWS and DevOps fundamentals.*
