![](./media/media/image2.png)

HOLOPS-1803
===========

# Task 0 Verify Lab Setup
----------------

In this task, you verify lab setup access

You session setup:

![](./media/media/dcloud-setup.png)

###  Access Information

(revise this section after dCloud setup is finalized)
        

### Access Lab Setup. 

1. Once you RDP to windows workstation, access NSO application VM:
    
    Click ![](./media/media/putty.png)from desktop, to open a putty connection to NSO VM

2. Expected result:

![](./media/media/nso-vm.png)

### Validate the setup

Follow the following instruction. 

1.  Check NSO version.

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

1.  Make sure NSO is running:

    ```
    [nso@nso ncs-run]$  ncs --status | head
    ```
    Sample output:
    
    ```
    [nso@nso ncs-run]$ ncs --status | head
    vsn: 4.5.0.1
    SMP support: yes, using 4 threads
    Using epoll: yes
    available modules:        
    ....
    ```
    
    **Note: if you get `connection refused`, start NSO application.  From your nso runtime directory:`/home/nso/ncs-run`, type `ncs`**
 

1.  Check pre-loaded packages in your NSO application.

    Check package version
    
    ```
    [nso@nso ncs-run]$ ncs_cli -u admin
    admin connected from 128.107.235.22 using ssh on cl-lab-211
    admin@ncs> show packages package package-version
    ```
    Sample output:
    
    ```
    nso@ncs> show packages package package-version
             PACKAGE
    NAME         VERSION
    ----------------------
    cisco-iosxr  6.2.9

    [ok][2020-12-11 13:55:52]

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
    weigang's note, checking iosxr versions here
    
    **Make sure the version of cisco-iosxr is 6.2.9 and the
    oper-status is up**

1.  Check the NSO instance contains 3 PE devices, asr9k0, asr9k1,
    asr9k2.

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
[	ok][2020-12-12 15:12:05]

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
     
2. Exit from cli

   ```
   admin@ncs> exit
   [nso@nso ncs-run]$
   ```
   
   
You have finished Task 0: Verify Lab Setup. Now you are ready to move on
to the next Task: Create a service package:

 [Task1 Create L2VPN Service Package](https://github.com/weiganghuang/HOLOPS-1803/blob/master/task1.md)
------

  

