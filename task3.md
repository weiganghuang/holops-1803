![](./media/media/image2.png)

HOLOPS-1803
===========

**[Lab Introduction](https://github.com/weiganghuang/HOLOPS-1803/blob/master/HOLOPS-1803.md)**

**[Previous Scenario](https://github.com/weiganghuang/HOLOPS-1803/blob/master/task2.md)**

**[Next Scenario (Optional)](https://github.com/weiganghuang/HOLOPS-1803/blob/master/task4.md)**


Scenario 4.	Service Discovery and Reconciliation: Reset Reference Count
-------------------------------------------------------------

As the first step of service discovery, the L2Vpn service instances were created in the previous scenario. To complete the discovery process, we must reconcile the pre-existing L2VPN services.

NSO uses an annotation tag of reference count to keep track of the configuration ownership. In this scenario, you will learn how to transfer the ownership of a pre-existing configuration from a device’s out-of-band to NSO by using the ref-count reset operation. Once the ref-count is reset, NSO can manage the lifecycle of the pre-existing L2VPN services. You will test by deleting the service instances and see the associated Bundle-Ether-sub interfaces being removed from the devices.

In this scenario, you will work on instance test2 to complete the service discovery/reconcile operations by resetting the reference count (ref-count).


### Check the ref-count of L2VPN Configuration

1.  Now we will check the ref-count of the Bundle Ether sub-interfaces. As you can see in the output, Bundle-Ether 100.2234 has backpointers that point to the service instance test2 (which we created previously). The value of ref-count is 2. This indicates that NSO service instance test2 is not the sole owner of the configuration.

	```
	admin@ncs% show devices device asr9k0 config cisco-ios-xr:interface Bundle-Ether-subinterface | display service-meta-data
	```
	Sample output displays the reference count and backpointer:
	
	![](./media/media/refcount1.png)
	
### Reset the ref-count of L2VPN configuration

In this procedure, you will transfer the configuration ownership from device to NSO through ref-count reset.

1. Reset the ref-count of Bundle-Ether 100.2234 by using the service L2Vpn re-deploy reconcile command.  This resets the value of ref-count, and service instance test2 will then be the sole owner of the configurations.

	```
	admin@ncs% request services L2Vpn test2 re-deploy reconcile
	[ok][2020-11-26 11:42:19]
  	[edit]
	```
  

1. Perform a device sync-from operation.
	```
	admin@ncs% request devices sync-from
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
	[ok][2020-12-29 09:20:11]

	[edit]

	```

1. Now let's check the ref-count. Notice the value of the ref-count attached with Bundle-Ether 100.2234 is 1, backpointer to test2.  
	
	```
	admin@ncs% show devices device asr9k0 config cisco-ios-xr:interface Bundle-Ether-subinterface | display service-meta-data
	```
	Sample output displays the reference count and backpointer of `test2` after reconciliation. Note the value of ref-count attached
    with `Bundle-Ether 100.2234` is 1, backpointer to `test2`:
	
	![](./media/media/refcount2.png)
  

### Try to Delete the Service Instance Created Previously

After re-setting the ref-count, the pre-existing L2VPN is reconciled, and NSO is managing the lifecycle of the reconciled service instance. In this procedure, you will see the correct behavior when we delete `test2`.

1.  Delete `test2` and notice that the output of the `commit dry-run outformat native` command contains the correct `no` statement to remove the Bundle Ether sub-interface 100.2234.

	Check service instance `test2`
    
    ```
    admin@ncs% show services L2Vpn test2
    order-number  L1111318;
    customer-name L_unitedhealth_318;
    pe-devices asr9k0 {
       Bundle-Ether 100;
       stag         2234;
    }
    [ok][2020-12-29 10:04:30]
    
    [edit]
    ```
    Delete `test2`:
    ```
    admin@ncs% delete services L2Vpn test2
    [ok][2020-12-29 11:34:32]
    [edit]
    ```
    
    Perform a commit dry-run to see the correct cli to remove the sub-interface: `no interface Bundle-Ether 100.2234 l2transport`
    
    ```
    admin@ncs% commit dry-run outformat native
    ```
    
    Expected output:
    
    ```
    native {
      device {
        name asr9k0
        data no interface Bundle-Ether 100.2234 l2transport
      }
    }
    [ok][2020-12-29 11:34:39]
    
    [edit]
    ```

1. Commit after viewing confirming the dry-run output.
   
   ```
   admin@ncs% commit
   Commit complete.
   [ok][2020-12-29 11:34:47]
  
   [edit] 
   ```

1. Now check the device model to confirm that Bundle-Ether 100.2234 no longer exists in asr9k0.
   
   ```
   admin@ncs% show devices device asr9k0 config cisco-ios-xr:interface Bundle-Ether-subinterface Bundle-Ether 100.2234
   ----------------------------------------------------- ------------------------------------------------------\^
   syntax error: element does not exist
   [error]$[2020-12-29 11:40:54]
  
   [edit]
   ```

1. Enter the following commands to return to the nso@nso prompt.

   ```
   admin@ncs% exit
   [ok][2019-06-06 14:03:16]
   admin@ncs> exit
   [nso@nso packages]$ cd
   [nso@nso ~]$

   ```

You have successfully finished all the required scenarios of this lab.

*	In Scenario 2, you learned how to generate NSO service packages.  You created a service package L2Vpn with service Yang model, and template xml to generate device configlet.  
*	From Scenarios 3 and 4, you learned how to perform service discovery with a brownfield network. You created L2Vpn instances from pre-existing device configurations, observed the issues, and reset the reference count to fully manage the lifecycle of L2Vpn service instances.

The next scenario shows how to extend the service discovery process from manual to automatic. It is recommended go through the scenario and learn how to create massive service instances from pre-existing configurations and reset the ref-count automatically.

**This concludes scenario 4. Continue with the next scenario: Create an NSO Action to Discover Pre-existing L2VPN Service Instances Automatically**

**[Next Scenario (Optional)](https://github.com/weiganghuang/HOLOPS-1803/blob/master/task4.md)**

**[Previous Scenario](https://github.com/weiganghuang/HOLOPS-1803/blob/master/task2.md)**

**[Lab Introduction](https://github.com/weiganghuang/HOLOPS-1803/blob/master/HOLOPS-1803.md)**
