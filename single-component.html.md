---
title: Understanding the Effects of Single Components on a PCF Upgrade
---

The [Resource Config page](../getstarted/index.html#configure-er) of Pivotal Elastic Runtime shows the 24 components that the Ops Manager Director installs.
You can specify the number of instances for 11 of the components.
We deliver the remaining 13 resources as single components, meaning that they have a preconfigured and unchangeable value of one instance.

In a single-component environment, upgrading can cause the deployment to experience downtime and other limitations because there is no instance redundancy.
Although this behavior might be acceptable for a test environment, you should configure the scalable components with editable instance values, such as HAProxy, Router, and DEA, for optimal performance in a production environment.

<p class="note"><strong>Note</strong>: A full Ops Manager upgrade may take close to two hours, and you will have limited ability to deploy an application during this time.</p>

### Summary of Component Limitations ###

The table lists components in the order that Ops Manager upgrades each component, and includes the following columns:

* **Scalable?**: Indicates whether the component has an editable value or a preconfigured and unchangeable value of one instance.
    <p class="note"><strong>Note</strong>: For components marked with a checkmark in this column, we recommend that you change the preconfigured instance value of 1 to a value that best supports your production environment. For more information on scaling a deployment, refer to the [Scaling Cloud Foundry topic](../concepts/high-availability.html).
* **Extended Downtime**: Indicates that the component is unavailable for up to five minutes during an Ops Manager upgrade.
* **Other Limitations and Information**: Provides the following information:
    * Component availability, behavior, and usage during an upgrade
    * Guidance on disabling the component before an upgrade</p>

<p class="note"><strong>Note</strong>: The table does not include the Run Smoke Tests and Run CF Acceptance Tests errands and the Compilation job. Ops Manager runs the errands after it upgrades the components, and creates compilation VMs as needed during the upgrade process.</p>

<table border="1" class="nice" >
<tr>
  <th><strong>Component</strong></th>
  <th><strong>Scalable?</strong></th>
  <th><strong>Extended Downtime</strong></th>
  <th><strong>Other Limitations and Information</strong></th>
</tr>
<tr>
  <td>HAProxy</td>
  <td>&#10003;</td>
  <td>&#10003;</td>
  <td></td>
</tr>
<tr>
  <td>NATS</td>
  <td>&#10003;</td>
  <td>&#10003;</td>
  <td></td>
</tr>
<tr>
  <td>etcd</td>
  <td></td>
  <td>&#10003;</td>
  <td></td>
</tr>
<tr>
  <td>Health Manager</td>
  <td>&#10003;</td>
  <td></td>
  <td>Ops Manager operators will see a 5 minute delay in app cleanup during an upgrade.</td>
</tr>
<tr>
  <td>NFS Server</td>
  <td></td>
  <td>&#10003;</td>
  <td>You cannot push, stage, or restart an app when an upgrade affects the NFS Server.</td>
</tr>
<tr>
  <td>Cloud Controller Database</td>
  <td></td>
  <td>&#10003;</td>
  <td></td>
</tr>
<tr>
  <td>Cloud Controller</td>
  <td>&#10003;</td>
  <td>&#10003;</td>
  <td>Your ability to manage an app when an upgrade affects the Cloud Controller depends on the number of instances that you specify for the Cloud Controller and DEA components.
If either of these components are single components, you cannot push, stage, or restart an app during the upgrade.</td>
</tr>
<tr>
  <td>Clock Global</td>
  <td></td>
  <td></td>
  <td></td>
</tr>
<tr>
  <td>Cloud Controller Worker</td>
  <td>&#10003;</td>
  <td>&#10003;</td>
  <td></td>
</tr>
<tr>
  <td>Router</td>
  <td>&#10003;</td>
  <td>&#10003;</td>
  <td></td>
</tr>
<tr>
  <td>Pivotal Ops Metrics Collector</td>
  <td>&#10003;</td>
  <td></td>
  <td>The Pivotal Ops Metrics tool is a JMX extension for Elastic Runtime that you can <a href="http://docs.pivotal.io/pivotalcf/customizing/deploy-metrics.html">install</a>. If you install this tool, Ops Manager operators will see a 5 minute delay in metrics collection during an upgrade.
<br />
You can disable this component before an upgrade to reduce the overall system downtime.</td>
</tr>
<tr>
  <td>UAA Database</td>
  <td></td>
  <td></td>
  <td></td>
</tr>
<tr>
  <td>UAA</td>
  <td>&#10003;</td>
  <td></td>
  <td>If a user has an active authorization token prior to performing an upgrade, the user can still log in using either a UI or the CLI.</td>
</tr>
<tr>
  <td>Login</td>
  <td>&#10003;</td>
  <td>&#10003;</td>
  <td></td>
</tr>
<tr>
  <td>Apps Manager Database</td>
  <td></td>
  <td></td>
  <td>You can disable this component before an upgrade to reduce the overall system downtime.</td>
</tr>
<tr>
  <td>MySQL Server</td>
  <td></td>
  <td></td>
  <td></td>
</tr>
<tr>
  <td>DEA</td>
  <td>&#10003;</td>
  <td>&#10003;</td>
  <td>Your ability to manage an app when an upgrade affects the DEA depends on the number of instances that you specify for the Cloud Controller and DEA components.
If either of these components are single components, you cannot push, stage, or restart an app during the upgrade.
<br />
The Apps Manager app and App Usage Service components for the Apps Manager Database run in a single DEA instance. You cannot use the Apps Manager or the CLI during the upgrade of the DEA. </td>
</tr>
<tr>
  <td>Doppler Server</td>
  <td></td>
  <td></td>
  <td>Ops Manager operators will see 2-5 minute gaps in logging.</td>
</tr>
<tr>
  <td>Loggregator Traffic Controller</td>
  <td></td>
  <td></td>
  <td>Ops Manager operators will see 2-5 minute gaps in logging.</td>
</tr>
<tr>
  <td>Push Apps Manager and Push App Usage Service errands</td>
  <td></td>
  <td></td>
  <td>These errands run scripts that connect the Apps Manager app and the App Usage Service components to the Apps Manager Database.
<br />
The Apps Manager app and App Usage Service components runs in a single DEA instance and the Apps Manager Database is a single component. If there is an upgrade issue with either Apps Manager Database instance or the DEA instance, the upgrade fails and Ops Manager will not run this errand.</td>
</tr>
</table>


