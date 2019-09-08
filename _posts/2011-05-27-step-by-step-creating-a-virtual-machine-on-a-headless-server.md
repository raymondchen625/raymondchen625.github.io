---
id: 343
title: 'Step by step: creating a virtual machine on a headless server'
date: 2011-05-27T11:26:22+00:00
author: raymondchen625
layout: post
guid: http://www.raymondchen.com/?p=343
permalink: /2011/05/27/step-by-step-creating-a-virtual-machine-on-a-headless-server/
original_post_id:
  - "343"
  - "1083"
categories:
  - Tech
---
<div>
  <div>
    <div>
      <h3>
        Step by step: creating a virtual machine on a headless server
      </h3>
    </div>
  </div>
</div>

The following instructions may give you an idea how to create a virtual machine on a headless server over a network connection. We will create a virtual machine, establish an RDP connection and install a guest operating system &#8212; all without having to touch the headless server. All you need is the following:

<div>
  <ol type="1">
    <li>
      VirtualBox on a server machine with a supported host operating system. The VirtualBox extension pack for the VRDP server must be installed (see the previous section). For the following example, we will assume a Linux server.
    </li>
    <li>
      An ISO file accessible from the server, containing the installation data for the guest operating system to install (we will assume Windows XP in the following example).
    </li>
    <li>
      A terminal connection to that host through which you can access a command line (e.g. via <code>ssh</code>).
    </li>
    <li>
      An RDP viewer on the remote client; see <a title="7.1.1. Common third-party RDP viewers" href="ch07.html#rdp-viewers">Section 7.1.1, “Common third-party RDP viewers”</a> above for examples.
    </li>
  </ol>
</div>

Note again that on the server machine, since we will only use the headless server, neither Qt nor SDL nor the X Window system will be needed.

<div>
  <ol type="1">
    <li>
      On the headless server, create a new virtual machine: <pre>VBoxManage createvm --name "Windows XP" --ostype WindowsXP --register</pre>
      
      <p>
        Note that if you do not specify <code>--register</code>, you will have to manually use the <code>registervm</code> command later.
      </p>
      
      <p>
        Note further that you do not need to specify <code>--ostype</code>, but doing so selects some sane default values for certain VM parameters, for example the RAM size and the type of the virtual network device. To get a complete list of supported operating systems you can use
      </p>
      
      <pre>VBoxManage list ostypes</pre>
    </li>
    
    <li>
      Make sure the settings for this VM are appropriate for the guest operating system that we will install. For example: <pre>VBoxManage modifyvm "Windows XP" --memory 256 --acpi on --boot1 dvd --nic1 nat</pre>
    </li>
    
    <li>
      Create a virtual hard disk for the VM (in this case, 10GB in size): <pre>VBoxManage createhd --filename "WinXP.vdi" --size 10000</pre>
    </li>
    
    <li>
      Add an IDE Controller to the new VM: <pre>VBoxManage storagectl "Windows XP" --name "IDE Controller"
      --add ide --controller PIIX4</pre>
    </li>
    
    <li>
      Set the VDI file created above as the first virtual hard disk of the new VM: <pre>VBoxManage storageattach "Windows XP" --storagectl "IDE Controller"
      --port 0 --device 0 --type hdd --medium "WinXP.vdi"</pre>
    </li>
    
    <li>
      Attach the ISO file that contains the operating system installation that you want to install later to the virtual machine, so the machine can boot from it: <pre>VBoxManage storageattach "Windows XP" --storagectl "IDE Controller"
      --port 0 --device 1 --type dvddrive --medium /full/path/to/iso.iso</pre>
    </li>
    
    <li>
      Start the virtual machine using VBoxHeadless: <pre>VBoxHeadless --startvm "Windows XP"</pre>
      
      <p>
        If everything worked, you should see a copyright notice. If, instead, you are returned to the command line, then something went wrong.</li> 
        
        <li>
          On the client machine, fire up the RDP viewer and try to connect to the server (see <a title="7.1.1. Common third-party RDP viewers" href="ch07.html#rdp-viewers">Section 7.1.1, “Common third-party RDP viewers”</a> above for how to use various common RDP viewers). <p>
            You should now be seeing the installation routine of your guest operating system remotely in the RDP viewer.</li> </ol> </div>