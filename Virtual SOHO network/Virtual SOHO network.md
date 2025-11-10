# Virtual SOHO network

## Overview

This document presents a step-by-step configuration of a SOHO network in a virtual environment. It does not cover the installation of the machines or software; installation guides are linked in the "Installations" section. A video tutorial covering the same content is also available at the end of this overview.

### Project Objectives
1. Configure the pfSense router and its necessary services (DNS, DHCP, Firewall).  
2. Connect the virtual machines to the virtual network.  
3. Verify connectivity between the machines.  
4. Isolate traffic between the virtual machines using aliases and firewall rules.

### Resources / Technologies
- VMware Workstation Pro  
- pfSense router (ISO)  
- Windows 11 VM (ISO)  
- Ubuntu 24.0.3 VM (ISO)
- Bash (for scripting)  
- PowerShell (for Windows configuration)

Full video of this project: [Watch on YouTube](https://youtu.be/IiSkOGPZcAg?si=_76tftqQx_HEKrUu)
