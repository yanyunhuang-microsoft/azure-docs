---
title: Manage snapshot policies in Azure NetApp Files | Microsoft Docs
description: Describes how to create, manage, modify, and delete snapshot policies by using Azure NetApp Files. 
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''

ms.assetid:
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 09/16/2021
ms.author: b-juche
---

# Manage snapshot policies in Azure NetApp Files

[Snapshots](snapshots-introduction.md) enable point-in-time recovery of volumes. You can schedule for [volume snapshots](snapshots-introduction.md) to be taken automatically by using snapshot policies. You can also modify a snapshot policy as needed, or delete a snapshot policy that you no longer need.  

## Create a snapshot policy 

A snapshot policy enables you to specify the snapshot creation frequency in hourly, daily, weekly, or monthly cycles. You also need to specify the maximum number of snapshots to retain for the volume.  

1.	From the NetApp Account view, click **Snapshot policy**.

    ![Screenshot that shows how to navigate to Snapshot Policy.](../media/azure-netapp-files/snapshot-policy-navigation.png)

2.	In the Snapshot Policy window, set Policy State to **Enabled**. 

3.	Click the **Hourly**, **Daily**, **Weekly**, or **Monthly** tab to create hourly, daily, weekly, or monthly snapshot policies. Specify the **Number of snapshots to keep**.  

    See [Resource limits for Azure NetApp Files](azure-netapp-files-resource-limits.md) about the maximum number of snapshots allowed for a volume. 

    The following example shows hourly snapshot policy configuration. 

    ![Screenshot that shows the hourly snapshot policy.](../media/azure-netapp-files/snapshot-policy-hourly.png)

    The following example shows daily snapshot policy configuration.

    ![Screenshot that shows the daily snapshot policy.](../media/azure-netapp-files/snapshot-policy-daily.png)

    The following example shows weekly snapshot policy configuration.

    ![Screenshot that shows the weekly snapshot policy.](../media/azure-netapp-files/snapshot-policy-weekly.png)

    The following example shows monthly snapshot policy configuration.

    ![Screenshot that shows the monthly snapshot policy.](../media/azure-netapp-files/snapshot-policy-monthly.png) 

4.	Click **Save**.  

If you need to create additional snapshot policies, repeat Step 3.
The policies you created appear in the Snapshot policy page.

If you want a volume to use the snapshot policy, you need to [apply the policy to the volume](snapshots-manage-policy.md#apply-a-snapshot-policy-to-a-volume). 

## Apply a snapshot policy to a volume

If you want a volume to use a snapshot policy that you created, you need to apply the policy to the volume. 

You cannot apply a snapshot policy to a destination volume in cross-region replication.  

1.	Go to the **Volumes** page, right-click the volume that you want to apply a snapshot policy to, and select **Edit**.

    ![Screenshot that shows the Volumes right-click menu.](../media/azure-netapp-files/volume-right-cick-menu.png) 

2.	In the Edit window, under **Snapshot policy**, select a policy to use for the volume.  Click **OK** to apply the policy.  

    ![Screenshot that shows the Snapshot policy menu.](../media/azure-netapp-files/snapshot-policy-edit.png) 

## Modify a snapshot policy 

You can modify an existing snapshot policy to change the policy state, snapshot frequency (hourly, daily, weekly, or monthly), or number of snapshots to keep.  
 
1.	From the NetApp Account view, click **Snapshot policy**.

2.	Right-click the snapshot policy you want to modify, then select **Edit**.

    ![Screenshot that shows the Snapshot policy right-click menu.](../media/azure-netapp-files/snapshot-policy-right-click-menu.png) 

3.	Make the changes in the Snapshot Policy window that appears, then click **Save**. 

## Delete a snapshot policy 

You can delete a snapshot policy that you no longer want to keep.   

1.	From the NetApp Account view, click **Snapshot policy**.

2.	Right-click the snapshot policy you want to modify, then select **Delete**.

    ![Screenshot that shows the Delete menu item.](../media/azure-netapp-files/snapshot-policy-right-click-menu.png) 

3.	Click **Yes** to confirm that you want to delete the snapshot policy.   

    ![Screenshot that shows snapshot policy delete confirmation.](../media/azure-netapp-files/snapshot-policy-delete-confirm.png) 

## Next steps

* [Troubleshoot snapshot policies](troubleshoot-snapshot-policies.md)
* [Resource limits for Azure NetApp Files](azure-netapp-files-resource-limits.md)
* [Learn more about snapshots](snapshots-introduction.md)