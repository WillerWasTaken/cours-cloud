# Subject of the OPS project on AWS

The goal of the project is to deploy a fully (almost) automated pipeline on AWS
using gitlab-CI for the pipeline along with several tools such as terraform and
ansible to deploy the applications onto the cloud.

## The applications

The dev team has provided for you a set of 2 applications:

- Written in Python3, the backend is an REST API that exposes a user resource
in JSON format.
- Written in NodeJS, the front consumes the beforementioned API to retrieve the
information of the user and displays it in a plain html file.

The whole code can be found here
https://github.com/WillerWasTaken/front-back-example.

## Fork the project and make it yours

Each group will have to fork the applications repository onto gitlab.com, the
created repository must be set to **private**. Add each members of the group.
Also **include [this guy](https://gitlab.com/WillerWasTaken)** (don't worry he
doesn't bite) in with a read access (at least `Reporter` role since the
repository is private) to your repository.

The entire code for the project will be stored here. You are free to organize
the code as you wish. At first the repository will look like this:
```
/your-repository
|-- back
|   `-- back_files.... <-- All the files necessary for the back app
|-- front
|   `-- front_files... <-- All the files necessary for the front app
```

Here's a possible files hierarchy with the ops code:
```
/your-repository
|-- .gitlab-ci.yml <-- All the ci code will go in this file
|-- back
|   `-- back_files.... <-- All the files necessary for the back app
|-- front
|   `-- front_files... <-- All the files necessary for the front app
|-- ops
|   `-- ansible
|       `-- ansible_files... <-- All the files used by ansible
|   `-- terraform
|       `-- terraform_files... <-- All the files used by terraform
```

## A step into the cloud: AWS

The goal is to have applications deployed in the cloud using `EC2` instances so
that the applications can be accessed from anywhere using the public IP of the
machine running the app.

Using AWS console, create a new VPC called `dev` with at least one subnet with
an internet access. A seperated VPC must be created, do not use the default VPC.
*VPC will allow to separate different environments logically*. Every created
resources will run in your VPC.


Create an `EC2` instance that will hold the applications. It should be a
`t2.micro` instance (free-tier eligible) that runs an `Ubuntu Server 18.04`
image. The **only** ports accessible from anywhere are TCP **22 and 80**.

By using `ssh`, connect to the instance to configure it manually first. Install
every dependencies needed by the 2 applications to run properly in the instance.
Copy the 2 applications in their dedicated folders in the `/opt` directory.
It should look like this:
```
/opt
|-- back
|   `-- back_files.... <-- All the files necessary for the back app
|-- front
|   `-- front_files... <-- All the files necessary for the front app
```

Create 2 systemd [services](https://www.freedesktop.org/software/systemd/man/systemd.service.html),
one called `front` that will launch the front and one called `back` that will
launch, you guessed it, the backend.  Enable the 2 services. *Enabling the
services allow them to be started at system boot, for instance after a reboot of the server.*

Once done, it should look like this:
```
$ systemctl status front
● front.service - Front server
   Loaded: loaded (/etc/systemd/system/front.service; enabled; vendor preset: enabled)
   Active: active (running) since Tue 2019-12-17 21:10:11 UTC; 47min ago
 Main PID: 9707 (node)
    Tasks: 10 (limit: 1152)
   CGroup: /system.slice/front.service
           └─9707 /usr/bin/node /opt/front/server.js

Dec 17 21:10:11 ip-192-168-0-218 systemd[1]: Started Front server.
Dec 17 21:10:12 ip-192-168-0-218 node[9707]: Backend url found in the configuration http://localhost:5000
Dec 17 21:10:12 ip-192-168-0-218 node[9707]: Starting server on port 3000.
$ systemctl status back
● back.service - Back server
   Loaded: loaded (/etc/systemd/system/back.service; enabled; vendor preset: enabled)
   Active: active (running) since Tue 2019-12-17 21:10:06 UTC; 47min ago
 Main PID: 9609 (python3)
    Tasks: 1 (limit: 1152)
   CGroup: /system.slice/back.service
           └─9609 /usr/bin/python3 /opt/back/server.py

Dec 17 21:10:06 ip-192-168-0-218 systemd[1]: Started Back server.
Dec 17 21:10:07 ip-192-168-0-218 python3[9609]: User name found in the configuration toto
Dec 17 21:10:07 ip-192-168-0-218 python3[9609]:  * Serving Flask app "server" (lazy loading)
Dec 17 21:10:07 ip-192-168-0-218 python3[9609]:  * Environment: production
Dec 17 21:10:07 ip-192-168-0-218 python3[9609]:    WARNING: This is a development server. Do not use it in a producti
Dec 17 21:10:07 ip-192-168-0-218 python3[9609]:    Use a production WSGI server instead.
Dec 17 21:10:07 ip-192-168-0-218 python3[9609]:  * Debug mode: off
Dec 17 21:10:07 ip-192-168-0-218 python3[9609]:  * Running on http://0.0.0.0:5000/ (Press CTRL+C to quit)
$ curl localhost:5000/user # request the backend
{"name":"toto"}
$ curl localhost:3000 # request the front

        <!DOCTYPE html>
        <html>
        <head>
            <meta charset="utf-8">
            <title>Simple front example</title>
        </head>
        <body>
            <div>Hello world</div>
            <div>The user name is toto</div>
        </body>
        </html>
```

In case of a crash of any applications, they must be restarted automatically.

> Hint: Systemd should help you do that just fine.

You should have this behavior:
```
$ sudo kill 9609 # this is the pid of the backend app (cf the output of systemctl status back)
$ systemctl status back # A new PID is available for the application
● back.service - Back server
   Loaded: loaded (/etc/systemd/system/back.service; enabled; vendor preset: enabled)
   Active: active (running) since Tue 2019-12-17 22:09:57 UTC; 7s ago
 Main PID: 10028 (python3)
    Tasks: 1 (limit: 1152)
   CGroup: /system.slice/back.service
           └─10028 /usr/bin/python3 /opt/back/server.py

Dec 17 22:09:57 ip-192-168-0-218 systemd[1]: back.service: Scheduled restart job, restart counter is at 1.
Dec 17 22:09:57 ip-192-168-0-218 systemd[1]: Stopped Back server.
Dec 17 22:09:57 ip-192-168-0-218 systemd[1]: Started Back server.
Dec 17 22:09:58 ip-192-168-0-218 python3[10028]: User name found in the configuration toto
Dec 17 22:09:58 ip-192-168-0-218 python3[10028]:  * Serving Flask app "server" (lazy loading)
Dec 17 22:09:58 ip-192-168-0-218 python3[10028]:  * Environment: production
Dec 17 22:09:58 ip-192-168-0-218 python3[10028]:    WARNING: This is a development server. Do not use it in a product
Dec 17 22:09:58 ip-192-168-0-218 python3[10028]:    Use a production WSGI server instead.
Dec 17 22:09:58 ip-192-168-0-218 python3[10028]:  * Debug mode: off
Dec 17 22:09:58 ip-192-168-0-218 python3[10028]:  * Running on http://0.0.0.0:5000/ (Press CTRL+C to quit)
```

You now have the 2 applications running in the machine but they are not yet
accessible. This is where the Reverse Proxy comes in. *The role of a Reverse
Proxy is to have a single entrypoint to your service while redirecting the
calls to the underlying applications*. Install nginx and modify its
configuration `/etc/nginx/nginx.conf` to proxy to the 2 applications.

The rules that must be implemented are the following:

- Redirect any request to `/api` to the backend application.</br>
Example: `localhost/api/user` redirects to `localhost:5000/user`
- Redirect the remaining requests towards the front.</br>
Example: `localhost/some/path` redirects to `localhost:3000/some/path`

> For debugging purposes it may help to activate debug log for nginx using the
> directive: `error_log /var/log/nginx/error.log debug;` as shown
> [here](https://docs.nginx.com/nginx/admin-guide/monitoring/debugging/).

You should then have the following result:
```
$ curl localhost # request redirected to the front

        <!DOCTYPE html>
        <html>
        <head>
            <meta charset="utf-8">
            <title>Simple front example</title>
        </head>
        <body>
            <div>Hello world</div>
            <div>The user name is toto</div>
        </body>
        </html>
$ curl localhost/api # request redirected to the back
{"msg":"Hello from the API application"}
$ curl localhost/api/user # request redirected to the back
{"name":"toto"}
```

At this point, if done correctly you will be able to access the 2 applications
from your browser by using the public IP of the `EC2`:

- `<public-ec2-ip>` access the front
- `<public-ec2-ip>/api/user` access the backend

## Automate infrastructure creation, Terraform to the recue

From now on, the goal is to automate everything you did in the previous step
using powerful automation tools that will do everything you just did with a
single command. *With such tools you will be able to create a brand new
environment identical to what you already have with little to no efforts*.

The role of `Terraform` is to create every AWS resources previously created
manually with the AWS web console. This is called the *provision* phase.
`Terraform` **does not** install the applications in the machine, this will be
the job of `Ansible` as seen later.

### What is expected

From a clean AWS architecture, without any instances nor any extra VPCs. Using
`terraform apply` you must be able to create a full infrastructure :
- A new VPC (and all related resources) with internet access
- An instance running inside this PC
- Security groups so that the instance can be reached by SSH and HTTP (port 80)
- A key pair used to connect to the instance, the key file used must be
**present locally in the machine launching the command**

After applying the code, the newly created instance must be reachable by SSH
using the key provided during the provision phase using `ssh ubuntu@<public-ip>`

The terraform code must be stored in the git repository in a dedicated folder.

### A few things to note

- **ALWAYS USE THE SAME TERRAFORM VERSION**, terraform is pretty sensible to the
version, the state file is bound to a particular so everyone must stick to the
same version for the duration of the project
- **TFSTATE FILE MUST BE MANIPULATED WITH CAUTION**, terraform stores all its
state in a dedicated file called `terraform.tfstate`. This is how terraform
knows which resources are managed by the code and what are their current states.
When using `terraform apply`, always rely upon the same `terraform.tfstate` so
that terraform knows the exact state of the infrastructure.<br/>
If you end up in a weird state where you don't know anymore what terraform
really manages, you can delete the state file and go to the AWS console to
delete every resources so you can start again from a clean state...
- Concerning the previous point, be careful to share the states file between the
people doing `terraform apply`. Here are a few options you can do to properly
work on terraform :
  - Rigorously exchange the state file so the one doing `terraform apply` always
uses the latest version. The state file can be added to the git repository
  - Always `apply` on the same computer
  - Work independently on separate AWS accounts so one's can code and test on
his own isolated environment
- Use default shared credentials file that is located at
`$HOME/.aws/credentials` as mentioned in the
[provider documentation](https://www.terraform.io/docs/providers/aws/index.html)
- Set the aws provider version as advised during the `terraform init`
- Add the `.terraform` folder to the `.gitignore`, it contains downloaded
terraform dependencies when launching `terraform init`
- In the documentation, the resources you will need can be found in `EC2` and
`VPC`
- Before saying "yes" when applying, check how resources are changed
- After applying, check the AWS console to see what is really done

## Ansible, a new way of scripting your deployment

Terraform takes care of all the provisioning of the infrastructure, but we now
only have plain vms with the default image running on it. The goal is to install
and run all the various applications in the deployed instance. Ansible is the
tool to accomplish just that.

### What is expected

From a new EC2 machine without anything installed, using
`ansible -i path/to/your/inventory_file app.yml` you must be able to install
the applications you setup previously using ssh in the EC2 instance.
- A front application as a systemd service
- A back application as a systemd service
- Nginx with the redirection rules mentioned previously
- Every applications running and answering in their respective ports (80, 3000,
  5000)
- Same installation directory (/opt/front, /opt/back), services enabling/restart
policy
- Basically the same state you had when installing the applications manually

For the code layout, a single playbook named `app.yml` using 3 roles
`front`, `back`, `nginx` are expected.

### A few things to note

- Note carefully every commands used to install the various applications in
order to report them using Ansible modules
- After application of the playbook, check the state of the vm using ssh
- Each time the instance is destroyed and then created again with terraform a
new public IP will be assigned, report it to your inventory file and update the
code base accordingly
- Be careful with the order of your commands, successive runs will modify the
state of the machine. Ansible may execute successfully because of the favorable
initial state of the instance. It is advised to run the whole playbook in a
fresh instance to check that modules are used in a coherent order
- **Bonus**: Tired of modifying the host file everytime the infra is recreated ?
You can use dynamic inventory to automatically retrieve the IP of the instance
(cf `aws_ec2` ansible plugins). This step will require more work and will
require you to adapt your CI pipeline accordingly
