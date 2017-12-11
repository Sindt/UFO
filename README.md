# UFO - Blog Entry

## Vagrant Essentiels : Hacks and Tips for developers

If you work with software, you’ve probably had the “It works on my machine!” problem. Luckily, these days there are some great tools, to ease this pain. Every developer, needs to know one of the most popular tools – Vagrant, and how Vagrant is useful to solve problems like “works on my machine” and at the same time lowers development environment setup time.

Vagrant is an open-source product for building and maintaining portable virtual development environments. These virtual systems are cost-efficient to use for developing your project and then disassembling it when you are done. When running a software application, you are also running all the libraries it uses, dependencies it has, and the operating system it’s hosted on. This is where the problems arise. It could be because you’re running a different operating system version or using a different tool set. 

Vagrant are based on the idea of “virtual” computing, where your application will run in a predictable and repeatable way and this guarantees, that you will feel comfortable, it will work on your co-workers pc, and the production server too. 

## What does Vagrant do?
Vagrant uses virtualization to run an operating system within another, a Virtual Machine. It makes it possible to easily copy the same configurations onto another machine. It allows you to configure a virtual machine in plain text and get it up and running with little trouble.

### Why Vagrant?
Vagrant matters a great deal, because if you are setting everything up manually, you end up spending a lot of time doing so everytime. In the interest of saving a lot of time and headaches, Vagrant is very useful. It has a low barrier to entry and gets you into development in sooner rather than later. The simplest use case is probably having multiple machines running on one machine with the exact same configurations and Vagrant lets you do this with a simple, plain-text file. 

With Vagrant, the deployment process is streamlined. Vagrant does this by having a .vagrantfile in which you specify exactly what you want and/or need for your VM. The .vagrantfile is a configuration file. The only real prerequisite is command line knowledge. 
Through the configuration file you can easily grab any operating system you want, and in our case we chose Ubuntu.

### How to use Vagrant
To use Vagrant you need a VM package installed and in our case that is Oracle’s VM VirtualBox. And of course you need Vagrant installed.
To set up a VM you need a .vagrantfile which tells Vagrant what to do.
We used Vagrant to overcome the problem of setting up and deploying a web server and database. 


![vagrant1](https://i.imgur.com/dw5jeUU.png)

We start by setting Vagrant to use this config to use digital ocean and setting our private key path

![vagrant2](https://i.imgur.com/bexvQsZ.png)

We then set the provider to digital ocean and set some settings for the web server, such as the operating system, the region and the size.

![vagrant3](https://i.imgur.com/O9jNhBT.png)

Then we setup a server for the database in the same way as the web server. 

With this config we have been able to easily deploy our web server and database. If we require more features, we can quite easily add it to this .vagrantfile and simply redeploy it. It also allows us to set up more machines with the exact same configuration to ensure the best compatibility between all servers.

Now we can do the following command to deploy the server(s) according to the configuration:

$ vagrant up

And access it like so:

$ vagrant ssh


### Hyper-V’s and the Blue Screen of Death!
As mentioned earlier, once you’ve used Vagrant (and another tool like Virtual Box) to set up a virtual server, you run a provisioning script the exact same way on each machine. But using Vagrant on a Windows environment is not always without complications.
<br>
**Expected behavior:**
<br>
Run Vagrant up, Virtual machine should be running
<br>

**Actual behavior:**
<br>
Blue Screen of death, after repairing disk it returns this error:

“There was an error while executing VBoxManage, a CLI used by Vagrant for controlling VirtualBox. The command and stderr is shown below.”
<br>
So, after you as a developer tries to google the error, we found out the Hyper-V in windows 8 and higher is the culprit! 

### Microsoft Hyper-V
Back in 2008, Microsoft introduced Hyper-V as a virtualization platform, and some tools uses this platform today. Hyper-V is a hybrid hypervisor, which is installed from OS, and during installation it redesigns the OS architecture and becomes just like a next layer on the physical hardware.

![hyperv](https://user-images.githubusercontent.com/11289686/33810342-d365716c-de03-11e7-9c61-ec3b0b4624c2.png)

But, enabling Hyper-V will cause VirtualBox, VMware, and any other virtualization technology to no longer work on Windows.
Fortunately, there are many solutions online, like:
Disabling the Hyper-V services, from the "Turn windows features on or off" Control panel setting.
 
Unfortunately, this solution is not reliable, because this version of Hyper-V is required, to run e.g. Docker Windows containers on Windows 10. This is independent of Docker for Windows. Of course, you could enable Hyper-V again whenever you need Docker to run, but that seems like a big deal to do every time, plus it’s guaranteed that your often will forget to disable the Hyper-V, resulting in a nasty BSOD.     

### Setting Up Docker With Vagrant
Vagrant is designed to work on Linux, Mac OS X, or Windows, but it can be used also with other providers. The underlying virtualization solutions are called providers. To work with Vagrant, you must have at least one provider, and one of the most popular and recommended are VirtualBox. Provisioning in Vagrant is the process of automatic installation and configuration of the system within during.

Vagrant comes with support out of the box for using Docker as a provider, and the provider behaves just like any other provider. The Docker provider does not require a “config.vm.box”. Since the "base image" for a Docker container is pulled from the Docker Index, the box does not add much value, and is optional for this provider.

On systems that cannot run Linux containers natively, such as Mac OS X or Windows, Vagrant automatically spins up a "host VM" to run Docker. This allows your Docker-based Vagrant environments to remain portable and still not being platform-depending.  
```ruby
Vagrant.configure("2") do |config|
  config.vm.provider "docker" do |d|
	d.image = " nginx"
  end
end
```
Vagrant can also automatically build and run images based on a local Dockerfile, all you need is to define “d.build_dir” in the provider.
In this way you do not need to install Docker on your local Windows PC, and even better, there are no need for Windows to be running the Docker Hyper-V service, meaning you won’t be facing any BSOD on running “Vagrant up”.   


 

