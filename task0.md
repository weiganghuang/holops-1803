![](./media/media/image2.png)

HOLOPS-1803
===========

**[Lab Introduction](https://github.com/weiganghuang/HOLOPS-1803/blob/master/HOLOPS-1803.md)**

**[Next Scenario](https://github.com/weiganghuang/HOLOPS-1803/blob/master/task1.md)**

# Scenario 1 Verify Lab Setup
--------------------


### Connecting to the Workstation

We will need use Remote Desktop to the workstation where you can access the lab server. There are two options. Option A requires Cisco AnyConnect Client and Remote Desktop client. Option B is using WebRdp.

#### Option A

1. Download Cisco Anyconnect if you don't have it already. Download url: [AnyConnect](https://software.cisco.com/download/home/286281283/type/282364313/release/4.9.06037)
2. From the list, download the pre-deployment version based on your OS:
	![](./media/media/anyconnect.png)
3. unzip the downloaded file and install

4. Get Remote Desktop Client if you don't have it already. 

	Urls: 
	
	[Remote Desktop RDP Client for Mac](https://apps.apple.com/au/app/microsoft-remote-desktop/id1295203466?mt=12)
		
	[Desktop for Windows](https://docs.microsoft.com/en-us/windows-server/remote/remote-desktop-services/clients/remote-desktop-clients)
	
5. Connect to AnyConnect VPN. From your dCloud session, click View -> Details, use the AnyConnect Credentials to connect.
	![](./media/media/anyconnect-creds.png)

6. Open your remote desktop RDP client, enter the following to connect:
	* IP Address for the remote PC: `198.18.133.252`
	* Credentials: `Administrator/C1sco12345` 

#### Option B 

1. From your dCloud session, click on Workstation icon, and then from the popup menu, click on Remote Desktop:

	![](./media/media/webrdp.png)


### Verify Lab Setup 

1. On the remote desktop, double-click the **nso@nso** click ![](./media/media/putty.png) icon to open a PuTTY connection to the NSO VM.
2. Enter the following commands to fully initialize all elements of the lab. The initialization takes approximately five minutes.


    ```
    [nso@nso ~]$ ansible-playbook ~/ansible/set-lab.yml
    ```
1.  Enter the following commands to verify the NSO version.

    ```
    [nso@nso ~]$ cd ncs-run
    [nso@nso ncs-run]$ pwd
    [nso@nso ncs-run]$ ncs --version
    ```
    Sample output:
    
    ```
    [nso@nso]$ cd ncs-run
    [nso@nso ncs-run]$ pwd
    /home/nso/ncs-run
    [nso@nso ncs-run]$ ncs --version
    4.5.0.1
    ```
 

1.  Enter the following commands to access the CLI and check the pre-loaded packages in your NSO application.
    
    ```
    [nso@nso ncs-run]$ ncs_cli -u admin
    admin connected from 198.18.133.252 using ssh on cl-lab-211
    admin@ncs> show packages package package-version
    ```
    Sample output:
    
    ```
    admin@ncs> show packages package package-version
                 PACKAGE
    NAME         VERSION
    ----------------------
    cisco-iosxr  6.2.9    
    ```
    
    Check package operation status:
    
    ```
    admin@ncs> show packages package oper-status
    ```
    
    Sample output:
    
    ```
    nso@ncs> show packages package oper-status
    packages package cisco-iosxr
    oper-status up
    [ok][2020-12-11 13:56:09]
  
    ```
    
    **Make sure the version of cisco-iosxr is 6.2.9 and the
    oper-status is up**

1.  Verify that the NSO instance contains three PE devices (asr9k0, asr9k1, asr9k2).

    ```
    admin@ncs> show devices brief
    ```
    Sample output:
    
    ```
    admin@ncs> show devices brief
	 NAME    ADDRESS    DESCRIPTION  NED ID
	 --------------------------------------------
	 asr9k0  127.0.0.1  -            cisco-ios-xr
	 asr9k1  127.0.0.1  -            cisco-ios-xr
	 asr9k2  127.0.0.1  -            cisco-ios-xr
    [ok][2020-12-12 15:12:05]

    ```

1.  Sync up the devices to bring the PE devices configuration into NSO’s
    device model.

     ```
     admin@ncs> request devices sync-from
     ```

     Sample output:
     
      ```
     admin@ncs> request devices sync-from
     sync-result {
    	device asr9k0
    	result true
	  }
	  sync-result {
    	device asr9k1
    	result true
     }
     sync-result {
    	device asr9k2
    	result true
	  }

     ```
     
2. Enter the following commands to return to the nso@nso prompt.

   ```
   admin@ncs> exit
   [nso@nso ncs-run]$
   ```

**This concludes scenario 1. Next Scenario: Create a service package, continue with to the next scenario: Create L2VPN Service Package**

**[Next Scenario](https://github.com/weiganghuang/HOLOPS-1803/blob/master/task1.md)**

**[Lab Introduction](https://github.com/weiganghuang/HOLOPS-1803/blob/master/HOLOPS-1803.md)**


  

