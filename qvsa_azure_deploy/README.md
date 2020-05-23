<h2>Deploy Qualys Virtual Scanner Appliance On Azure</h2>
<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fqualys-qvsa-v22-marketplace%2Fazuredeploy.json" target="_blank">
    <img src="http://azuredeploy.net/deploybutton.png"/>
</a>
<a href="http://armviz.io/#/?load=https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fqualys-qvsa-v22-marketplace%2Fazuredeploy.json" target="_blank">
    <img src="http://armviz.io/visualizebutton.png"/>
</a>
<h3>Deploy using azcli</h3>

```shell
az group deployment create --debug --verbose --name <deployment name> --template-file azure_deploy.json --resource-group <resource-group-name>
```
```shell
# type '?' to see help text
Please provide string value for 'perscode' (? for help): ?
Input: Personalization code
Description: Qualys scanner personalization code, exactly 14 digits
Learn more: https://www.qualys.com/docs/qualys-virtual-scanner-appliance-user-guide.pdf
```
```shell
# with parameter file
az group deployment create --debug --verbose --name <deployment name> --template-file azure_deploy.json --resource-group <resource-group-name> --parameters @<path_to_json_parameter_file>
```
<h3>Deploy using PowerShell</h3>

```shell
New-AzResourceGroupDeployment -ResourceGroupName <resource-group-name> -TemplateFile azure_deploy.json
```
```shell
# with parameter file
New-AzResourceGroupDeployment -ResourceGroupName <resource-group-name> -TemplateFile azure_deploy.json -TemplateParameterFile <path_to_json_parameter_file>
```

<table style="width:100%">
  <tr>
    <th>Parameter</th>
    <th>Input Value</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>location</td>
    <td>Location identifier</td>
    <td>Location identifier for deployment (example:- eastus)</td>
  </tr>
  <tr>
    <td>scannerName</td>
    <td>Scanner VM name</td>
    <td>VM name on Azure can be 1-64 character long and may contain alphanumerics, underscores, periods, and hyphens.<br> <a href="https://docs.microsoft.com/en-us/azure/azure-resource-manager/management/resource-name-rules#microsoftcompute" target="_blank">Learn more</a></td>
  </tr>
  <tr>
    <td>scannerVmSize</td>
    <td>Scanner VM size</td>
    <td>Any one of the following<br><code>Standard_A1_v2, Standard_A2_v2, Standard_A4_v2, Standard_A8_v2, Standard_B1ms, Standard_B2s, Standard_B2ms, Standard_B4ms, Standard_D2a_v4, Standard_D4a_v4, Standard_D2as_v4, Standard_D4as_v4, Standard_DC1s_v2, Standard_DC2s_v2, Standard_DC4s_v2, Standard_D1_v2, Standard_D2_v2, Standard_D3_v2, Standard_DS1_v2, Standard_DS2_v2, Standard_DS3_v2, Standard_D2_v3, Standard_D4_v3, Standard_D2s_v3, Standard_D4s_v3, Standard_F2s_v2, Standard_F4s_v2, Standard_F8s_v2, Standard_D11_v2, Standard_DS11_v2, Standard_E2a_v4, Standard_E2as_v4, Standard_E2_v3, Standard_E2s_v3</code><br><a href="https://docs.microsoft.com/en-us/azure/virtual-machines/linux/sizes" target="_blank">Learn more</a></td>
  </tr>
  <tr>
    <td>perscode</td>
    <td>Personalization code</td>
    <td>Qualys scanner personalization code, exactly 14 digits<br><a href="https://www.qualys.com/docs/qualys-virtual-scanner-appliance-user-guide.pdf" target="_blank">Learn more</a></td>
  </tr>
  <tr>
    <td>proxy</td>
    <td>Proxy string</td>
    <td>Optional proxy configuration for scanner<br>Sample formats-<br><code>proxy://&lt;host&gt;:&lt;port&gt; (No auth proxy)<br>proxy://&lt;user&gt;:&lt;password&gt;@&lt;host&gt;:&lt;port&gt; (Auth proxy)<br>proxy://&lt;domain\\user&gt;:&lt;password&gt;@&lt;host&gt;:&lt;port&gt; (Auth proxy with domain user)</code><br><a href="https://discussions.qualys.com/docs/DOC-5725-scanning-in-microsoft-azure-using-resource-manager-arm" target="_blank">Learn more</a></td>
  </tr>
  <tr>
    <td>ImageResourceIdOrVhdUri</td>
    <td>
      <ol>
        <li>qVSA image resource id</li>
        <li>Storage account vhd blob</li>
        <li>'global_marketplace'</li>
      </ol>
    </td>
    <td>
      <ol>
        <li>Deploy qVSA scanner from existing image</li>
        <li>Storage account uri to qVSA disk vhd blob</li>
        <li>String identifier to deploy scanner from global marketplace image</li>
      </ol>
    </td>
  </tr>
  <tr>
    <td>osDiskType</td>
    <td>OS disk type from allowed values</td>
    <td> Any one of the following<br><code>Premium_SSD, StandardSSD_LRS, Standard_LRS</code><br>Premium Disk (Premium_SSD) is recommended due to their production performance but only available with selected VM sizes<br><a href="https://docs.microsoft.com/en-us/azure/virtual-machines/windows/disks-types" target="_blank">Learn more</a></td>
  </tr>
  <tr>
    <td>bootDiagStorageAccNameOrUri</td>
    <td>
      <ol>
        <li>Unique name for new storage account</li>
        <li>Existing storage account uri</li>
        <li>Leave it empty</li>
      </ol>
    </td>
    <td>
      <ol>
        <li>Ensure that name string is unique as storage account names are globally unique</li>
        <li>Template will use existing storage account</li>
        <li>Template will disable boot diagnostics (Not recommended practice)</li>
      </ol>
    </td>
  </tr>
  <tr>
    <td>virtualNetworkNewOrExisting</td>
    <td>
      <ol>
        <li>'new' for new virtual network with default configurations</li>
        <li>resource_id:subnet_name where resource_id is of the existing virtual network</li>
      </ol>
    </td>
    <td>Virtual network options for scanner vm</td>
  </tr>
  <tr>
    <td>publicIpNewOrExisting</td>
    <td>
      <ol>
        <li>'new' for new public ip with default configurations</li>
        <li>resource_id of the existing public ip instance</li>
      </ol>
    </td>
    <td>Public IP options for scanner vm</td>
  </tr>
  <tr>
    <td>newVirtualNetworkCustomObject</td>
    <td>vNet custom property object</td>
    <td>Values in this object will override default values. See "Custom Virtual Network And Public IP Parameters" section for more information.</td>
  </tr>
  <tr>
    <td>newPublicIpCustomObject</td>
    <td>Public Ip custom property object</td>
    <td>Values in this object will override default values. See "Custom Virtual Network And Public IP Parameters" section for more information.</td>
  </tr>
</table>
<h3>Custom Virtual Network And Public IP Parameters</h3>
<p>Template deploys new virtual network and public ip instances with default paramters layed inside template. If needed these variables can be overridden with custom values. Paramters newVirtualNetworkCustomObject and newPublicIpCustomObject do exactly that. Each except a custom json object representing vNET and Public-IP configuration respectively.</p>

<p>Sample for more details: </p>

```json
{
  "vNetAddressPrefixes": ["10.0.0.0/24"],
  "subnet": {
    "name": "scanner-subnet",
    "addressPrefix": "10.0.0.0/24"
  }
}
```
```json
{
  "sku": "Basic",
  "publicIpAllocationMethod": "Dynamic",
  "idleTimeoutInMinutes": 30
}
```