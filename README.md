<h1> Project: SOC-LAB – Threat Detection & Enrichment with Azure Sentinel</h1>

  <div class="section">
    <h2>Overview:</h2>
    <p>This project sets up a functional SOC environment using <strong>Azure Sentinel</strong>, <strong>Log Analytics</strong>, and <strong>GeoIP enrichment</strong> to detect and visualize suspicious login activity across virtualized infrastructure.</p>
  </div>

<div class="section">
  <h2>Languages & Utilities Used:</h2>
  <ul>
    <li><strong>Azure CLI</strong> – for provisioning cloud infrastructure</li>
    <li><strong>Kusto Query Language (KQL)</strong> – for querying log data in Sentinel</li>
    <li><strong>Microsoft Sentinel</strong> – SIEM and SOAR integration</li>
    <li><strong>Log Analytics</strong> – centralized logging and analysis</li>
    <li><strong>Watchlists</strong> – for GeoIP-based enrichment</li>
  </ul>
</div>

<div class="section">
  <h2>Environments Used:</h2>
  <ul>
    <li><strong>macOS Terminal</strong> – for running Azure CLI commands and scripting</li>
    <li><strong>Windows Remote Desktop</strong> – to access Azure-hosted Windows VMs for simulation and testing</li>
    <li><strong>Azure Portal</strong> – for visual resource management and Sentinel configuration</li>
    <li><strong>Log Analytics Workspace</strong> – for querying and managing log data</li>
  </ul>
</div>

<div class="walkthrough-step">
  <h3>Step 1: Resource Group and Virtual Network Setup</h3>
  <p>The project began with provisioning a new Azure resource group named <code>RG-SOC-LAB</code> to logically contain and organize all security-related resources. Using the Azure CLI from macOS Terminal, I deployed a virtual network (<code>VNET-SOC-LAB</code>) with a default subnet in the <code>eastus2</code> region. This forms the foundation of the simulated enterprise network and ensures future security controls, such as NSGs and flow logs, can be applied within a structured environment.</p>
</div>

<div class="walkthrough-step">
  <h3>Step 2: Querying Login Attempts in Microsoft Sentinel</h3>
  <p>With Sentinel enabled on my Log Analytics workspace (<code>SOC-LAB-LOG</code>), I began the threat detection process by crafting a custom KQL query to surface failed login attempts (Event ID 4625). The query extracted IP addresses from raw event data and prepared them for GeoIP enrichment. Although the workbook visualization and enrichment component encountered issues, executing this query validated log ingestion and demonstrated my ability to parse and manipulate complex log structures inside Microsoft Sentinel.</p>
</div>

</body>
</html>
