# UFO - Blog Entry

## Vagrant Essentiels : Hacks and Tips for developers

If you work with software, you’ve probably had the “It works on my machine!” problem. Luckily, these days there are some great tools, to ease this pain. Every developer, needs to know one of the most popular tools – Vagrant, and how Vagrant is useful to solve problems like “works on my machine” and at the same time lowers development environment setup time.

Vagrant is an open-source product for building and maintaining portable virtual development environments. These virtual systems are cost-efficient to use for developing your project and then disassembling it when you are done. When running a software application, you are also running all the libraries it uses, dependencies it has, and the operating system it’s hosted on. This is where the problems arise. It could be because you’re running a different operating system version or using a different tool set. 

Vagrant are based on the idea of “virtual” computing, where your application will run in a predictable and repeatable way and this guarantees, that you will feel comfortable, it will work on your co-workers pc, and the production server too. 

### Hyper-V’s and the Blue Screen of Death!
As mentioned earlier, once you’ve used Vagrant (and another tool like Virtual Box) to set up a virtual server, you run a provisioning script the exact same way on each machine. But using Vagrant on a Windows environment is not always without complications.
<br>
**Expected behavior:**
<p>Run Vagrant up, Virtual machine should be running</p>
<br>
**Actual behavior:**
<p>Blue Screen of death, after repairing disk it returns this error:</p>

“There was an error while executing VBoxManage, a CLI used by Vagrant for controlling VirtualBox. The command and stderr is shown below.”
<br>
So, after you as a developer tries to google the error, we found out the Hyper-V in windows 8 and higher is the culprit! 

### Microsoft Hyper-V
Back in 2008, Microsoft introduced Hyper-V as a virtualization platform, and some tools uses this platform today. Hyper-V is a hybrid hypervisor, which is installed from OS, and during installation it redesigns the OS architecture and becomes just like a next layer on the physical hardware.

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


 

