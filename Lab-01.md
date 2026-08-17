# IBM MoRE Lab Guide

## IBM Modernized Runtime Extension for Java (MoRE) — Hands-on Lab Guide

> Creating a Managed Liberty Cluster and deploying a Java 17 application within a WebSphere Network Deployment cell.

---

## Table of Contents

1. [Overview](#overview)
2. [Connecting to Your Environment](#connecting-to-your-environment)
3. [Preparing the Environment](#preparing-the-environment)
4. [Optional: Install a Java 8 Application on WebSphere Application Server](#optional-install-a-java-8-application-on-websphere-application-server)
5. [Creating a Managed Liberty Cluster](#creating-a-managed-liberty-cluster)
6. [Deploying a Java 17 Application into a Liberty Cluster](#deploying-a-java-17-application-into-a-liberty-cluster)

---

## Overview

IBM Modernized Runtime Extension for Java (MoRE) allows businesses to leverage existing investments in the IBM WebSphere Network Deployment (WAS ND) administrative console by enabling Java 8 and Java 17 (including Jakarta EE 10) deployments to be managed under a single operational model.

By preserving the traditional WebSphere scripting model, IBM MoRE eliminates the need for a significant operating model change, enabling application updates without disrupting business operations. Ensure continuity, reduce disruption, and modernize with ease.

By using MoRE, you will be able to simplify the process of modernization by reducing the risk and complexity associated with migrating to a new runtime environment.

> **Prerequisite:** This guide assumes the lab environment has already been requested, provisioned, and is in **Ready** status.

---

## Connecting to Your Environment

1. Go to TechZone, locate your requested environment, and confirm it is in **Ready** status.

   ![TechZone environment list showing Ready status](images/tz-ready-status.jpg)

   > **Tip:** You can also monitor your Slack messages — you will receive a notification once your environment is ready.

   ![Slack notification for environment ready](images/slack-notification.jpg)

2. Click the twistie (▶) to the left of the environment name to expand detailed information.

   ![Expanding environment details with twistie](images/twistie-expand.jpg)

3. Click the URL on the right side of the screen to access the environment console.

   ![URL link to environment console](images/env-console-url.jpg)

4. Click the **Console** button.

   ![Console button](images/console-button.jpg)

5. Click **Connect with RDP**.

   ![Connect with RDP button](images/rdp-button.png)

6. Click **No Thanks** when prompted.

   ![No Thanks prompt](images/no-thanks-prompt.jpg)

Your environment is now ready to use.

> **Copy & Paste:** To copy and paste between your desktop and the virtual environment, use the **Send Text** button in the ribbon at the top of the browser screen.
>
> ![Send Text button in the browser ribbon](images/send-text-button.jpg)

> **Session Timeout:** Your session will close after an inactivity period. To log in again, go back to step 4, click the eye icon in the **Password** field to reveal the password, copy it, then return to your environment. Ensure the cursor is in the password box, click **Send Text**, paste the password, and press **Enter**.
>
> ![Eye icon to reveal password](images/eye-icon-password.png)
>
> ![Session login flow](images/session-login.jpg)

---

## Preparing the Environment

1. Open a terminal window to execute commands.

   ![Open terminal window](images/open-terminal.png)

   ![Terminal window open](images/terminal-open.png)

2. Open IBM Installation Manager to confirm installed components:

   ```bash
   /home/itzuser/usr/IBM/IM/eclipse/IBMIM
   ```

   ![IBM Installation Manager](images/ibm-im.jpg)

   Click **Uninstall** to review installed components. **Do not uninstall anything.**

   ![Uninstall screen showing installed components](images/im-uninstall-screen.jpg)

3. Clone the repository with content and supporting materials for the labs:

   ```bash
   git clone https://github.com/sebasporro/MoRE.git
   ```

4. Start the Deployment Manager:

   ```bash
   /home/itzuser/usr/IBM/WebSphere/AppServer/bin/startManager.sh
   ```

5. Start the Node Manager:

   ```bash
   /home/itzuser/usr/IBM/WebSphere/AppServer/profiles/AppSrv01/bin/startNode.sh
   ```

6. Connect to the admin console at:

   ```
   http://localhost:9043/ibm/console
   ```

   ![Admin console login page](images/admin-console-login.jpg)

   If prompted with a security warning, click **Advanced** and then **Accept Risk and Continue**.

   ![Security warning — Advanced option](images/security-warning.jpg)

7. Log in with the following credentials:

   | Field    | Value      |
   |----------|------------|
   | Username | `wasadmin` |
   | Password | `password` |

   ![Admin console credentials screen](images/admin-console-creds.jpg)

8. Navigate to **Servers → All Servers**.

   ![Servers menu navigation](images/servers-menu.png)

   ![All Servers view](images/all-servers.png)

9. Select both servers and click **Start**.

   ![Selecting both servers](images/select-servers.jpg)

   ![Start button for selected servers](images/start-servers.jpg)

10. Monitor the startup process until it is complete.

    ![Server startup progress](images/startup-progress.png)

    ![Startup complete](images/startup-complete.png)

---

## Optional: Install a Java 8 Application on WebSphere Application Server

1. Navigate to **Applications → New Application → New Enterprise Application**.

   ![New Enterprise Application menu](images/new-app-menu.jpg)

   ![New Enterprise Application dialog](images/new-app-dialog.jpg)

2. Click **Browse**, select `modresorts-1.0-java8.war` (see the path shown on screen), and click **Open**.

   ![Browse for modresorts-1.0-java8.war](images/browse-war.png)

   ![File selection dialog](images/file-selection.png)

   ![Selected WAR file](images/war-selected.jpg)

3. Click **Next**.

   ![Next button](images/next-button.jpg)

4. Select **Fast Path** and click **Next**.

   ![Fast Path selection](images/fast-path.jpg)

5. Leave all options at their defaults, scroll to the bottom of the page, and click **Next**.

   ![Default options screen](images/default-options.png)

   ![Scrolled to bottom, Next button](images/default-options-next.png)

6. Select both servers and the module below them, then click **Apply**.

   ![Server and module selection](images/server-module-selection.jpg)

7. Click **Next**.

   ![Next button after Apply](images/next-after-apply.jpg)

8. Leave the options as shown on screen and click **Next**.

   ![Options screen](images/options-screen.png)

   ![Next button](images/next-button-2.png)

9. Review the summary screen and click **Finish**.

   ![Summary review screen](images/summary-review.jpg)

10. Click **Finish**.

    ![Finish button confirmation](images/finish-button.jpg)

11. Click **Save**.

12. Navigate to **Applications → All Applications**.

    ![All Applications menu](images/all-apps-menu.png)

    ![All Applications list](images/all-apps-list.png)

13. Select the installed application and click **Submit Action**.

    ![Submit Action for installed app](images/submit-action.jpg)

14. Refresh the screen and confirm the application status shows **Green** (started).

    ![Application status Green](images/app-status-green.jpg)

15. Open a browser and verify connectivity at:

    ```
    http://localhost:9080/resorts
    ```

    ![ModResorts application running in browser](images/modresorts-browser.jpg)

---

## Creating a Managed Liberty Cluster

Create a cluster with two managed Liberty servers.

1. In the admin console, navigate to **Servers → Clusters → WebSphere Application Server Clusters**.

   ![Clusters navigation menu](images/clusters-menu.png)

   ![WebSphere Application Server Clusters](images/was-clusters.jpg)

2. Click **New**.

   ![New cluster button](images/new-cluster.jpg)

3. In the **Cluster Name** field, enter `MLS`, then click **Next**.

   ![Cluster name MLS](images/cluster-name-mls.jpg)

4. In the **Member Name** field, enter `Liberty1` and select `default-managed-liberty-server` as the template, then click **Next**.

   ![Liberty1 member name and template](images/liberty1-member.jpg)

5. Click **Add Member**, enter `Liberty2` as the member name, then click **Next**.

   ![Adding Liberty2 member](images/liberty2-member.jpg)

6. Review the final configuration and click **Finish**.

   ![Final cluster configuration review](images/cluster-final-review.jpg)

7. Click **Save**.

   ![Save cluster configuration](images/save-cluster.png)

   ![Save confirmation](images/save-confirmation.png)

8. Select the **MLS** cluster and click **Start**. Wait for the startup process to complete.

   ![Starting MLS cluster](images/start-mls.jpg)

   ![Cluster startup in progress](images/cluster-startup-progress.jpg)

9. Click **Refresh** and confirm the cluster has started.

   ![Cluster started — refresh view](images/cluster-refresh.png)

   ![Cluster running confirmation](images/cluster-running.png)

10. Navigate to **Servers → All Servers** and confirm both Liberty servers are up and running.

    ![All Servers showing Liberty1 and Liberty2 running](images/liberty-servers-running.jpg)

---

## Deploying a Java 17 Application into a Liberty Cluster

1. Navigate to **Applications → New Application → New Enterprise Application**.

   ![New Enterprise Application for Java 17](images/new-app-java17.jpg)

2. Click **Browse**, select `server-info.war` (note the path shown on screen), and click **Open**.

   ![Browse for server-info.war](images/browse-server-info.png)

   ![server-info.war selected](images/server-info-selected.png)

3. Select **WebSphere Liberty** as the target runtime and click **Next**.

   ![WebSphere Liberty target runtime selection](images/liberty-runtime.jpg)

4. Select **Fast Path** and click **Next**.

   ![Fast Path selection](images/fast-path-java17.jpg)

5. Click **Next**, then select both the cluster and its servers along with the module below them. Click **Apply**.

   ![Cluster, servers, and module selection](images/cluster-server-module.jpg)

6. Review the results and click **Next**.

   ![Results review](images/results-review.png)

   ![Next after review](images/next-after-review.png)

7. Click **Next**.

   ![Next button](images/next-button-3.jpg)

8. Enter `server-info` as the **Context Root** and click **Next**.

   ![Context Root set to server-info](images/context-root.jpg)

9. Review the details and click **Finish**.

   ![Final details review](images/final-details.png)

   ![Finish button](images/finish-java17.png)

10. Click **Save**.

    ![Save application changes](images/save-app.jpg)

11. Navigate to **Applications → All Applications**.

    ![All Applications list](images/all-apps-java17.jpg)

12. Select `server-info.war` and click **Submit Action** to start the application.

    ![Submit Action for server-info.war](images/submit-server-info.jpg)

13. Navigate to **Server Type → Web Servers**.

    ![Web Servers navigation](images/web-servers-nav.png)

    ![Web Servers list](images/web-servers-list.png)

14. Select `webserver1` and click **Generate Plug-in**.

    ![Generate Plug-in for webserver1](images/generate-plugin.jpg)

15. Select `webserver1` again and click **Propagate Plug-in**.

    ![Propagate Plug-in for webserver1](images/propagate-plugin.jpg)

16. Review the completion status.

    ![Plug-in propagation completion](images/plugin-propagation.png)

    ![Completion status confirmed](images/plugin-complete.png)

17. Verify application connectivity at:

    ```
    http://localhost:1080/server-info
    ```

    ![server-info app running on port 9081 — Liberty1](images/server-info-liberty1.jpg)

    > **Note:** The connection port is `9081`, which corresponds to server **Liberty1**.

### Testing High Availability

Navigate to **Servers → All Servers** and stop **Liberty1**. Once the server is stopped (shown in red in the admin console), return to the `server-info` application and click **Refresh**. The application reloads and is now served by **Liberty2** listening on port `9082`.

![server-info app failover to Liberty2 on port 9082](images/server-info-liberty2.jpg)

Navigate back to **Servers → All Servers** and restart **Liberty1** to complete the lab. Once the server is started, refresh the app and confirm it is served again by **Liberty1** on port `9081`.

### Optional: Configure Round-Robin Load Balancing

To disable sticky sessions and enable round-robin load balancing between the two application servers:

1. Click on the `server-info` application.
2. Under the **Web Module Properties** section, click **Session Management**.
3. Check the box for **Override Session Management** to detach the application from session affinity.
4. Save the configuration and test the application across multiple browser refreshes to confirm round-robin routing.

---

*Lab Guide for IBM MoRE — Modernized Runtime Extension for Java.*
