<h1> Project: SOC-LAB – Threat Detection & Enrichment with Azure Sentinel</h1>

  <div class="section">
    <h2>Overview:</h2>
    <p>This project sets up a functional SOC environment using <strong>Azure Sentinel</strong>, <strong>Log Analytics</strong>, and <strong>GeoIP enrichment</strong> to detect and visualize suspicious login activity across virtualized infrastructure.</p>
  </div>
<hr>
<div class="section">
  <h2> Languages & Utilities Used:</h2>
  <ul>
    <li><strong>Azure CLI</strong> – Infrastructure provisioning via Cloud Shell</li>
    <li><strong>Bash</strong> – Shell scripting for resource automation</li>
    <li><strong>KQL (Kusto Query Language)</strong> – Custom queries for log analysis in Sentinel</li>
    <li><strong>Microsoft Sentinel</strong> – SIEM platform for threat detection and workbook dashboards</li>
    <li><strong>Log Analytics</strong> – Central log repository with query capabilities</li>
    <li><strong>Azure Monitor Agent (AMA)</strong> – Data connector for Windows Security Events</li>
    <li><strong>CSV Watchlists</strong> – GeoIP enrichment for attacker geolocation</li>
    <li><strong>Windows RDP</strong> – Remote access to VM for honeypot configuration</li>
    <li><strong>macOS Terminal</strong> – Used to run connectivity tests and basic pings</li>
  </ul>
</div>

<hr>
<div class="section">
  <h2> Environments Used:</h2>
  <ul>
    <li><strong>Azure Cloud Shell</strong> – for provisioning and managing cloud infrastructure via CLI (resource groups, VNets, NSGs, etc.)</li>
    <li><strong>Azure Portal</strong> – used extensively for deploying resources, configuring Microsoft Sentinel, managing connectors, and visual monitoring</li>
    <li><strong>Log Analytics Workspace</strong> – served as the centralized log repository to store and analyze telemetry data</li>
    <li><strong>macOS Terminal</strong> – utilized to run Azure CLI commands and perform external network tests (e.g., ping VM)</li>
    <li><strong>Windows Remote Desktop App (macOS)</strong> – enabled secure remote access to the honeypot VM for simulation and configuration tasks</li>
    <li><strong>Windows Server (Azure VM)</strong> – acted as the honeypot environment, configured to receive and log inbound connection attempts</li>
  </ul>
</div>

<hr>
<div class="walkthrough-step">
  <h3>Step 1: Resource Group and Virtual Network Setup</h3>
  <p>The project began with provisioning a new Azure resource group named <code>RG-SOC-LAB</code> to logically contain and organize all security-related resources. Using the Azure Cloud Shell, I deployed a virtual network (<code>VNET-SOC-LAB</code>) with a default subnet in the <code>eastus</code> region. This forms the foundation of the simulated enterprise network and ensures future security controls, such as NSGs and flow logs, can be applied within a structured environment.</p>
  <img src="https://imgur.com/yOJR6R6.png" alt="Resource Group and VNet Setup" style="float: left; width: 500px; margin-top: 15px; border: 1px solid #ccc; border-radius: 5px;">
</div>

<hr>
<div class="walkthrough-step">
  <h3>Step 2: Deploying the Honeypot Virtual Machine</h3>
  <p>
    Next, I deployed a Windows virtual machine within the <code>RG-SOC-LAB</code> resource group and connected it to the previously created virtual network.
    This VM serves as a honeypot, designed to simulate a vulnerable system that can attract unauthorized login attempts.
    It provides the telemetry needed to analyze suspicious behavior and failed login activity later in the project.
  </p>
  <img src="https://imgur.com/7ivX8fu.png" alt="VM Deployment Screenshot" style="float: left; max-width: 100%; margin-top: 15px; border: 1px solid #ccc; border-radius: 5px;">
</div>

<hr>
<div class="walkthrough-step">
  <h3>Resource Group Deployment Confirmation</h3>
  <p>
    To validate that all components were properly deployed, I captured a screenshot of the <code>RG-SOC-LAB</code> resource group. 
    This view confirms that all core resources—such as the VM (<code>CORP-INC-EAST</code>), public IP, network interface, NSG, OS disk, and virtual network—were successfully created and are ready for configuration and monitoring.
  </p>
  <img src="https://imgur.com/DM0oKeA.png" alt="Resource Group Confirmation Screenshot" style="float: left; max-width: 100%; margin-top: 15px; border: 1px solid #ccc; border-radius: 5px;">
</div>

<hr>
<div class="walkthrough-step">
  <h3>Step 3: Modifying NSG Rules to Allow Inbound Traffic</h3>
  <p>
    To simulate a vulnerable environment and encourage malicious traffic for monitoring purposes, I edited the inbound security rules for the <code>CORP-INC-EAST</code> NSG. 
    All traffic was allowed through by setting the rule to accept connections from any source, any protocol, and any port. 
    This step intentionally reduces security posture to attract real-world login attempts and threats that can later be detected and analyzed using Sentinel.
  </p>
  <img src="https://imgur.com/f05HxOs.png" alt="Inbound NSG Rules Screenshot" style="float: left; max-width: 100%; margin-top: 15px; border: 1px solid #ccc; border-radius: 5px;">
</div>

<hr>
<div class="walkthrough-step">
  <h3>Step 4: Remote Access & Firewall Configuration</h3>
  <p>After deploying the VM, I used the Windows Remote Desktop app to connect to it using the assigned public IP. Once inside the Windows environment, I accessed the firewall settings and disabled protections across all profiles—Domain, Private, and Public. This increased the VM’s exposure, effectively making it a soft target for external threats, which was essential for testing detection and visibility through Sentinel.</p>
  <div style="display: flex; align-items: flex-start; gap: 12px; margin-top: 15px;">
   <img src="https://imgur.com/lvZUmtG.png" alt="RDP App Login" style="width: 38%; border: 1px solid #ccc; border-radius: 5px;">
    <img src="https://imgur.com/l6Y8tA6.png" alt="Windows Firewall Disabled" style="width: 44%; border: 1px solid #ccc; border-radius: 5px;">
  </div>

<hr>
  <div class="walkthrough-step">
  <h3>Step 5: External Connectivity Test</h3>
  <p>With the VM’s firewall fully disabled, I opened the macOS Terminal and initiated a ping to the public IP of the VM. This test served as a quick verification that the machine was accessible over the internet, validating the exposure required for threat monitoring in the SOC lab setup.</p>
  <img src="https://imgur.com/ROVvZJy.png" alt="Ping from macOS Terminal" style="width: 42%; margin-top: 15px; border: 1px solid #ccc; border-radius: 5px;">
</div>

<hr>
<div class="walkthrough-step">
  <h3>Step 6: Log Analytics Workspace + Sentinel Integration</h3>
  <p>I created a new Log Analytics Workspace that acts as the centralized repository for all telemetry and log data. Afterward, I connected this workspace to Microsoft Sentinel, enabling SIEM capabilities such as correlation rules, threat detection, and custom dashboards.</p>
  <img src="https://imgur.com/pADgxHR.png" alt="Sentinel and Log Analytics Setup" style="width: 42%; margin-top: 15px; border: 1px solid #ccc; border-radius: 5px;">
</div>

<hr>
<div class="walkthrough-step">
  <h3>Step 7: Windows Security Events + AMA Connector Setup</h3>
  <p>To begin collecting and analyzing login activity, I configured the ingestion of <strong>Windows Security Events</strong>, which track authentication attempts, user logons, and policy changes on a host. This is critical for identifying suspicious behavior. I also enabled the <strong>Windows Security Events via AMA (Azure Monitoring Agent)</strong> connector from the Microsoft Sentinel "Open connector page" to ensure log forwarding from the VM to the SIEM pipeline.</p>
  <div style="display: flex; gap: 20px; margin-top: 15px;">
    <img src="https://imgur.com/7wo63qt.png" alt="Download Security Events" style="width: 48%; border: 1px solid #ccc; border-radius: 5px;">
    <img src="https://imgur.com/4M6P4oA.png" alt="Open Connector Page" style="width: 48%; border: 1px solid #ccc; border-radius: 5px;">
  </div>
</div>

<hr>
<div class="walkthrough-step">
  <h3>Step 8: Querying Logs with KQL to Detect Failed Login Attempts</h3>
  <p>Next, I used the <strong>Kusto Query Language (KQL)</strong> interface within Microsoft Sentinel to begin analyzing incoming logs. KQL is the query language used in Azure to search and analyze large volumes of data in Log Analytics. You can access the KQL editor by navigating to <em>Microsoft Sentinel &gt; Logs</em> within your Azure portal. I queried the <code>SecurityEvent</code> table to surface failed login attempts, which confirmed that the VM was acting as a honeypot — attracting real-world intrusion attempts from external actors or automated bots.</p>
  <img src="https://imgur.com/2GJB733.png" alt="KQL Query Showing Failed Logins" style="max-width: 25%; margin-top: 15px; border: 1px solid #ccc; border-radius: 5px;">
</div>

<hr>
<div class="walkthrough-step">
  <h3>Step 9: Creating a GeoIP Watchlist in Microsoft Sentinel</h3>
  <p>To enrich the collected log data with geographic context, I created a custom watchlist in Microsoft Sentinel titled <code>geoip</code>. This watchlist included a CSV dataset of known IP address ranges along with their associated latitude, longitude, city, and country details. The purpose of this was to map failed login attempts and other suspicious traffic to physical locations. This step is essential for identifying potential threat actors by region and is a key part of threat intelligence workflows in SOC operations.</p>
  <img src="https://imgur.com/lJUHewP.png" alt="GeoIP Watchlist Creation" style="max-width: 30%; margin-top: 15px; border: 1px solid #ccc; border-radius: 5px;">
</div>

<hr>
<div class="walkthrough-step">
  <h3>Step 10: Validating GeoIP Integration with KQL Query</h3>
  <p>After uploading the GeoIP watchlist, I returned to the Log Analytics workspace and executed a KQL query using the <code>_GetWatchlist("geoip")</code> function. This confirmed that the CSV data was successfully loaded and accessible. By visualizing entries from the watchlist, I could verify that the enrichment layer was functioning as expected — showing city and country data for external IP addresses attempting to access the network.</p>
  <img src="https://imgur.com/Rwi0W1X.png" alt="GeoIP Query Results in KQL" style="max-width: 30%; margin-top: 15px; border: 1px solid #ccc; border-radius: 5px;">
</div>

<hr>
<div class="walkthrough-step">
  <h3>Conclusion & Final Thoughts</h3>
  <p>While the final workbook visualization wasn’t captured in the screenshots, it served as the culminating phase of the SOC-LAB environment — converting enriched telemetry into actionable geographic intelligence. By using a custom workbook linked to GeoIP data, the project aimed to surface global trends in suspicious login activity, offering valuable threat visibility.</p>

  <p>This hands-on buildout demonstrates my ability to design and deploy a full-stack detection environment in Azure. From initial infrastructure provisioning to log collection, enrichment, and real-time analysis using custom KQL, this lab showcases a practical approach to threat detection and security monitoring in the cloud.</p>

  <p>Next steps will include expanding the environment to support alert automation, incident correlation, and external threat intelligence feeds to build a more robust and proactive SOC pipeline.</p>
</div>

<hr>
<div class="section">
  <h2>🔗 Acknowledgments</h2>
  <p>This lab was inspired by <a href="https://youtu.be/g5JL2RIbThM?si=_D16NdPskIyhOYxH" target="_blank" rel="noopener noreferrer">Josh Makador’s SOC Lab tutorial on YouTube</a>. His walkthrough provided valuable guidance on setting up Microsoft Sentinel and building a simulated detection environment from scratch.</p>
</div>

</body>
</html>
